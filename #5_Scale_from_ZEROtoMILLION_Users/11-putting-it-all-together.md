# Stage 11: Putting It All Together — 0 to 1 Million Users

This is the capstone of the entire series. Every architectural decision we've made was a specific solution to a specific scaling pain point that emerged at a particular traffic level.

---

### The Evolution — One Page Summary

| Stage | Users | Problem Solved | Solution Added |
|---|---|---|---|
| **1** | 1 – 100 | Starting point | Single server (everything on 1 machine) |
| **2** | 100 – 5K | DB starves app server | Separate DB onto its own server |
| **3** | 5K – 50K | 1 machine not enough | Vertical scale DB; horizontally scale app |
| **4** | 50K – 200K | Can't add servers without routing | Load Balancer + multiple servers |
| **5** | 200K – 500K | DB reads overwhelmed | Primary + Read Replicas |
| **6** | 500K – 2M | DB queries too slow | Redis Cache + CDN |
| **7** | Any point | Sessions break | Stateless servers + Redis session store |
| **8** | 1M+ global | High latency + no redundancy | Multi-region data centers |
| **9** | Any point | Slow tasks block API | Message Queues (Kafka/RabbitMQ) |
| **10** | 1M+ writes/sec | Write bottleneck | DB Sharding / NoSQL |

---

## The Complete Architecture at 1 Million Users

```text
════════════════════════════════════════════════════════════════════════
                     USERS (GLOBAL)
════════════════════════════════════════════════════════════════════════
        Asia Users      US Users        EU Users
            │               │               │
            ▼               │               ▼
   ┌──────────────┐         │     ┌──────────────┐
   │  CDN Edge    │         │     │  CDN Edge    │
   │  (Mumbai)    │         │     │  (London)    │
   │ Static Files │         │     │ Static files │
   └──────────────┘         │     └──────────────┘
            │               │               │
     ┌──────┘          GeoDNS           └──────┐
     ▼                 Routes               ▼
┌──────────┐    ┌──────────────────┐    ┌──────────┐
│ Mumbai   │    │   US-East Data   │    │ London   │
│   DC     │    │     Center       │    │   DC     │
│          │    │                  │    │          │
│ [LB]     │    │ [Load Balancer]  │    │ [LB]     │
│ [App ×N] │    │ [App #1][App #2] │    │ [App ×N] │
│ [Redis]  │    │     [App #3]     │    │ [Redis]  │
│ [DB Rep] │    │   [Redis Cache]  │    │ [DB Rep] │
└──────────┘    │                  │    └──────────┘
     ▲          │ [Message Queue]  │          ▲
     │          │ (Kafka Cluster)  │          │
     │          │                  │          │
     │          │  [Worker Pool]   │          │
     │          │  (Async Jobs)    │          │
     │          │                  │          │
     └──────────│ [Primary DB]─────│──────────┘
                │ [DB Replica ×3]  │   Replication stream
                └──────────────────┘
                         │
              [DB Shard 0][DB Shard 1][DB Shard 2]
              (once writes overwhelm primary)
════════════════════════════════════════════════════════════════════════
```

![Complete 1 Million User Architecture](../../assets/architecture/complete_million_user_architecture.png)

---

## Step-by-Step: How a Request Flows Through This System

**Scenario:** User Alice in Mumbai visits QuickBite to view the menu and place an order.

1. **DNS Resolution:**
 GeoDNS returns IP of Mumbai Load Balancer (lowest latency).
2. **CDN Loading:** Static assets (logo, CSS) served from Mumbai edge node (5ms).
3. **API Request:** Alice clicks "Menu." Request hits Mumbai LB → App Server.
4. **Cache Hit:** App server checks Redis. Result found. Return JSON in 0.5ms.
5. **Order Placed:** Alice submits order. App server writes to Primary DB and publishes to Kafka. Returns "Accepted" [YES].
6. **Async Jobs:** Workers pick up Kafka signal to send email and deduct inventory in background.
7. **DB Replication:** Write to Primary is streamed to replicas in Mumbai and London.

---

### Key Design Principles

1. **Separate Concerns:** Tiers are independent (App, DB, Cache, Jobs).
2. **Eliminate SPOFs:** Redundancy at every level.
3. **Scale Horizontally:** Clone stateless services; don't just buy bigger machines.
4. **Cache Aggressively:** The best request never touches the database.
5. **Async Slow Tasks:** Avoid blocking the user; use queues for work > 100ms.
6. **Design for Failure:** Assume anything can break. Automate failover.

---

### Interview Cheat Sheet: "Design a System for 1M Users"

When asked this in an interview, walk through these phases:

1. **Clarify:** Read/write ratio? Global or regional? What type of data?
2. **Start Simple:** "I'll start with a single server, then evolve."
3. **Separate DB:** "I'll move the DB to its own server for independent scaling."
4. **Add LB + Scale App:** "Multiple stateless app servers behind a load balancer."
5. **Add Replicas:** "Read replicas for the DB since reads dominate."
6. **Add Cache:** "Redis cache in front of DB. CDN for static assets."
7. **Go Multi-Region:** "GeoDNS + multiple data centers for global latency and HA."
8. **Add Message Queue:** "Async jobs (email, video processing) go through Kafka."
9. **Shard if Needed:** "If write throughput exceeds Primary capacity, we shard."

This structured answer demonstrates that you understand **incremental, justified evolution**.

---

### Common HLD Interview Questions

**Q1: Picking the SINGLE most impactful optimization for load?**
*Answer:* **Caching.** Most traffic is reads. Redis can reduce DB load by 90% with minimal changes.
*Example:* An "Ecommerce" site during a sale experiences 100x traffic. Caching product details in Redis prevents the database from locking up under millions of identical queries.

**Q2: System handles 1k users but fails at 10k. Diagnostic approach?**
*Answer:* Measure first. CPU/RAM maxed? Scale servers. DB slow? Check indexes/cache. Bandwidth high? Add CDN.
*Example:* Using "Datadog," we see DB disk I/O at 100%. We run `EXPLAIN` and find a missing index on the `user_orders` table. Adding it fixes the 10k user crash instantly.

**Q3: Ensuring zero data loss on primary crash?**
*Answer:* Synchronous replication to at least one hot replica with automatic failover.
*Example:* In "AWS RDS Multi-AZ," every write is confirmed by a standby in another building before the user gets a "Success." If the primary building loses power, the standby becomes primary with 0% data loss.
