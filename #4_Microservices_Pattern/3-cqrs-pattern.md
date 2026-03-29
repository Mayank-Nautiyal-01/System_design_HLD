# CQRS — Command Query Responsibility Segregation -[important]

## The Core Problem: One Model Serving Two Masters

In a traditional service, a single data model (and a single database) handles both:
- **Writes:** "Place this order", "Update user profile", "Transfer $500"
- **Reads:** "Show me all orders for user ID 45 sorted by date, with product names and seller ratings"

These two workloads have **fundamentally opposing requirements**:

| Dimension        | Write (Command) Side                        | Read (Query) Side                            |
|------------------|---------------------------------------------|----------------------------------------------|
| **Goal**         | Enforce strict business rules & consistency | Return data as fast as possible              |
| **Data shape**   | Normalized (3NF) to prevent anomalies       | Denormalized / flat for fast reads           |
| **Volume**       | Usually lower                               | Usually 10-100x higher                       |
| **Optimization** | Transactional integrity, ACID              | Caching, indexing, pre-aggregation           |
| **Scaling**      | Scale vertically                            | Scale horizontally aggressively              |

Forcing both operations to share the same normalized model results in slow, complex reads (joins across many tables) and a database that cannot scale either workload optimally.

**CQRS** is the pattern that acknowledges these are two different problems and solves them with two different architectures.

## The CQRS Solution: Split Everything in Two

CQRS mandates a strict separation between two types of operations:

- **Commands** → Operations that **change state** (write, update, delete). They are routed to the **Write Model** (backed by a normalized, transactional database like PostgreSQL). Commands return no data — they only succeed or fail.
- **Queries** → Operations that **return data** (read only). They are routed to the **Read Model** (backed by a database optimized for the specific read workload — e.g., Elasticsearch, Redis, a flat denormalized SQL table, or a MongoDB read replica).

### CQRS Architecture Visualized:
```text
                             [ Client ]
                           /            \
                          /              \
              [ Command Handler ]    [ Query Handler ]
              (Write Side)           (Read Side)
                    |                       |
                    v                       v
           [ Write DB ]             [ Read DB ]
           (PostgreSQL -            (Elasticsearch /
            normalized)              Redis / Replica
                    |                - denormalized)
                    |                       ^
                    |   [ Event Bus /       |
                    └── Kafka / CDC ───────►┘
                        (Sync write DB
                         changes to
                         read DB async)
```

The **Event Bus** (Kafka or Change Data Capture / CDC tools like Debezium) is the synchronization bridge. Every time a write is committed to the Write DB, an event is published. The **Read Model Updater** (a simple consumer service) listens for this event and updates the Read DB accordingly.

This means the Read DB is **eventually consistent** with the Write DB. This is an explicit, accepted trade-off.

## A Concrete Example: E-Commerce Order History Page

**Without CQRS:**
To render a user's order history page, a single query must JOIN five tables:
`orders` ⟶ `order_items` ⟶ `products` ⟶ `sellers` ⟶ `delivery_status`

This is a slow, expensive query that scales badly. The same database handles millions of order history page views AND thousands of new orders being written per minute.

**With CQRS:**
- **Write side (PostgreSQL):** Handles new orders in a normalized, ACID-compliant schema.
- **Read side (Elasticsearch or denormalized table):** A pre-computed, flat document is stored for each user, containing all order data, product names, seller info, and delivery status already joined and indexed. The order history page makes a **single, instant, index lookup** instead of a five-table JOIN.
- **Sync:** When a new order is placed (Write DB commit), a Kafka event triggers the Read Model Updater to insert the prepared flat document into Elasticsearch.

## CQRS and Event Sourcing (The Natural Companion)

CQRS is frequently paired with **Event Sourcing**. In Event Sourcing, instead of storing the *current state* of an entity (e.g., `account_balance = 500`), you store the **entire history of events** that led to that state:

```
AccountCreated { balance: 0 }
MoneyDeposited { amount: 1000 }
MoneyWithdrawn { amount: 300 }
MoneyDeposited { amount: 200 }
-- Replay all → current balance: 900
```

The Write DB becomes an **append-only event log** (Kafka or a specialized event store). The Read Models are built by replaying and projecting these events into purpose-built views. This provides a built-in audit trail and the ability to rebuild any read model from scratch at any time.

---

## Full System Architecture

### ❌ WITHOUT CQRS — One Database Doing Everything (Gets Slow)

```text
  Imagine Instagram with ONE database handling EVERYTHING.

  1,000 users posting (WRITE)      10,000,000 users scrolling feed (READ)
         │                                        │
         └──────────────────┬─────────────────────┘
                            │
                            ▼
               ┌────────────────────────┐
               │     ONE Big Database   │
               │  (trying to do both)   │
               │                        │
               │  posts table           │
               │  users table           │
               │  likes table           │
               │  comments table        │
               └────────────────────────┘
                            │
              To show ONE person's feed, the database must:
              JOIN posts + users + likes + comments tables
              → This is SLOW. Under high traffic, everyone's feed is slow.
              → Writes also slow down because the DB is busy serving reads.
```

---

### ✅ WITH CQRS — Two Separate Systems, Each Built for Its Job

```text
  Split into two completely separate paths:

  USER WRITES A POST             USER READS THEIR FEED
  (Command side)                 (Query side)
         │                              │
         ▼                              ▼
  ┌──────────────────┐          ┌──────────────────────────────┐
  │   Write Database  │          │   Read Database               │
  │   (PostgreSQL)    │          │   (Redis / fast cache)        │
  │                   │          │                               │
  │   Stores posts    │          │   Stores pre-built feeds      │
  │   in proper       │          │   ready to serve instantly    │
  │   organized       │          │                               │
  │   tables          │          │   user_99_feed: [             │
  │                   │          │     post A, post B, post C    │
  │   Focused on:     │          │   ]  ← already joined, ready  │
  │   data safety     │          │                               │
  │   & correctness   │          │   Focused on: SPEED           │
  └──────────┬────────┘          └──────────────────────────────┘
             │                              ▲
             │                              │
             │  Every time someone posts,   │
             │  a background worker reads   │
             └──────the new post and ───────┘
                   updates everyone's
                   feed in the Read DB.
                   (happens automatically, takes ~1 second)
```

![CQRS Architecture](../../assets/architecture/cqrs_architecture.png)

**The big idea:** Posting (writing) and reading the feed are two very different jobs. CQRS gives each job its own dedicated database — one built for safety, one built for speed. It's like having a chef who only cooks, and a waiter who only serves. Each is perfect at their one job.



**Q1: When would you introduce CQRS into a system that is currently using a traditional single-model approach?**
*Answer:* CQRS introduces significant architectural complexity (two models, synchronization logic, eventual consistency) and should not be applied universally. The right triggers are: (1) Read and write workloads are clearly disproportionate (e.g., 100:1 reads vs. writes) and competing for the same database resources, causing performance degradation. (2) The shape of data needed for reads is fundamentally different from the normalized shape required for writes (many complex JOINs). (3) You need to scale read replicas independently from the write master.
*Example:* A social media platform's "news feed" workload (reads) is 1000x its "post creation" workload (writes). The feed must be assembled from data across users, posts, reactions, and comments tables. CQRS makes the feed a pre-computed, denormalized snapshot in Redis, while writes continue to use a normalized PostgreSQL database for data integrity.

**Q2: What is the key trade-off you explicitly accept when implementing CQRS with an asynchronous synchronization bus?**
*Answer:* **Eventual Consistency**. Because the Write DB and Read DB are synchronized asynchronously via an event bus (e.g., Kafka), there is a brief window — typically milliseconds to a few seconds — where a user who just placed an order may refresh their Order History page and not yet see it, because the Read Model has not yet been updated. This is an explicit, known, and acceptable trade-off for the massive performance and scalability gains on the read side. The system designer must identify which reads are "stale-tolerant" and which require strong consistency (e.g., checking account balance before a transfer must always read from the Write DB directly).
