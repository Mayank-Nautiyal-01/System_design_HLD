# Which Microservices Pattern Should You Use? — Decision Guide

A quick reference to choose the right pattern for the right problem. These patterns are **not mutually exclusive** — a production system typically uses several of them simultaneously.

---

## Pattern Overview At a Glance

| Pattern | Core Problem Solved | Key Trade-off |
|---|---|---|
| **Circuit Breaker** | Cascading failures across service calls | Returns stale/fallback data instead of real-time data |
| **Saga** | Distributed transactions without a shared DB | Eventual consistency; rollbacks are explicit, not automatic |
| **CQRS** | Read and write workloads with conflicting requirements | Two models to maintain; eventual consistency on reads |
| **Strangler Fig** | Safely migrating a live monolith to microservices | Slow (months/years); dual-maintenance period during migration |
| **Sidecar / Service Mesh** | Cross-cutting infrastructure concerns across many services | Operational overhead; adds a new infrastructure layer |

---

## Decision Flowchart

```text
                        What is your problem?
                              |
         ┌────────────────────┼─────────────────────────┐
         |                    |                          |
         v                    v                          v
  "A downstream       "I need a multi-step        "My reads and writes
  service keeps        business transaction         are growing at very
  crashing my          across multiple              different speeds /
  entire system"       microservices"               need different DBs"
         |                    |                          |
         v                    v                          v
  CIRCUIT BREAKER           SAGA                      CQRS
         
         ┌────────────────────┬─────────────────────────┐
         |                    |                          
         v                    v                          
  "I have a legacy      "Every service needs
   monolith and          mTLS, retries, tracing
   need to migrate       and I don't want to
   it safely"            write it 50 times"
         |                    |                          
         v                    v                          
  STRANGLER FIG         SIDECAR / SERVICE MESH   
```

---

## Situational Guide: When to Use Each

### 1. Circuit Breaker

**✅ Use when:**
- Your service makes synchronous HTTP/gRPC calls to downstream services
- A downstream failure should NOT crash your service
- You want instant fallback behavior instead of waiting 30 seconds for a TCP timeout
- You have non-critical dependencies (recommendations, analytics, notifications) that your core flow can proceed without

**❌ Do NOT use when:**
- The downstream call is truly mandatory and there is no valid fallback (e.g., checking auth before any action) — fail explicitly instead
- Simple internal function calls within the same service (no network hop = no need)

**Real-world examples:** Netflix homepage (Recommendation Service fails → fallback to Top 10), any payment retry system, any feed generation service.

---

### 2. Saga Pattern

**✅ Use when:**
- A single business operation must update data across **2+ microservices**, each with its own private database
- You cannot use a single ACID transaction (because there is no shared DB)
- The business logic has a well-defined compensating action for each step (e.g., "ReserveStock" → "ReleaseStock")

**Use Choreography when:**
- The flow has 2-3 steps and the services are stable
- You want zero central coordinator dependency
- Teams are independent and can own their own event contracts

**Use Orchestration when:**
- The flow has 4+ steps
- You need a single, auditable record of the transaction's state
- Debugging, monitoring, and re-driving failed transactions is important

**❌ Do NOT use when:**
- You only have one service / one DB — just use a normal ACID transaction
- The business operation cannot tolerate any window of inconsistency (extremely rare in practice — if so, reconsider your service boundaries)

**Real-world examples:** E-commerce order placement (Order → Inventory → Payment → Delivery), hotel booking systems (Reserve Room → Charge Card → Confirm Booking), airline ticketing.

---

### 3. CQRS

**✅ Use when:**
- Your read load is **significantly higher** (10x-1000x) than your write load
- The shape of data needed for reads requires joining many tables and is totally different from the normalized write shape
- You are already using an event-driven architecture (Kafka/event sourcing pairs naturally)
- You want to scale read replicas independently from the write primary
- You need purpose-built read stores (e.g., Elasticsearch for full-text search of the same data that is stored in PostgreSQL)

**❌ Do NOT use when:**
- Your system is small with balanced read/write loads — CQRS adds two databases, synchronization logic, and eventual consistency complexity for no benefit
- Your reads always require the absolute latest data (e.g., checking an account balance before a debit — always read from Write DB)
- Your team size is small — CQRS doubles the codebase surface area for the data layer

**Real-world examples:** Social media news feeds (Twitter, Facebook), e-commerce product catalogs, financial dashboards, reporting systems.

---

### 4. Strangler Fig

**✅ Use when:**
- You have a legacy monolith that needs to be modernized but cannot be taken offline
- The monolith has clear, well-bounded logical modules (Users, Payments, Inventory) that can be extracted individually
- The business requires the system to stay live during the migration (always true in practice)
- You want the ability to roll back any individual migration step independently

**❌ Do NOT use when:**
- The monolith is so deeply entangled that there are no clean seams to extract (requires refactoring the monolith first to create boundaries)
- The system is small enough that a complete rewrite over 2-3 weeks is feasible and low risk
- You are building a new system from scratch — start microservices-first instead

**Real-world examples:** Amazon (Obidos monolith → AWS microservices), eBay, Netflix, virtually every major company that modernized from 2010-2020.

---

### 5. Sidecar / Service Mesh

**✅ Use when:**
- You have **10+ microservices** in a Kubernetes cluster, making per-service network infrastructure unmanageable
- You need **mTLS encryption** for all pod-to-pod communication without modifying any service code
- You need **distributed tracing** across many services (see the full request journey end-to-end)
- You want to do **canary deployments** or traffic splitting without code changes
- Your services are written in **different languages** (impossible to share a single library)

**❌ Do NOT use when:**
- You have a small number of services (< 5-6) — the Istio/Linkerd control plane is significant operational overhead that is not justified at small scale
- Your team does not have Kubernetes/infrastructure expertise — a Service Mesh adds a complex new operational layer
- You are not on Kubernetes — Service Meshes are designed specifically for the Kubernetes environment

**Real-world examples:** Lyft (built Envoy specifically for this), Google, Airbnb, any organization running hundreds of microservices on Kubernetes.

---

## Patterns That Work Together: Common Combinations

| Combination | Why it works |
|---|---|
| **Circuit Breaker + Saga** | Saga steps that make synchronous calls wrap those calls in Circuit Breakers to prevent a failing downstream service from blocking the entire orchestrator |
| **CQRS + Saga** | The Saga writes to Write DBs; CQRS projects those writes to Read DBs via Kafka. The write and read models are cleanly separated |
| **CQRS + Event Sourcing** | The Write DB is an immutable event log; Read Models are built by replaying events. Natural pair |
| **Sidecar + Circuit Breaker** | Circuit breaking can be moved entirely to the Envoy sidecar layer (configured in Istio's Control Plane), removing the need for Resilience4j / Polly libraries inside every service |
| **Strangler Fig + all others** | During a Strangler Fig migration, each extracted microservice is built with the appropriate patterns from day one. The monolith is not touched |

---

## Complexity vs. Benefit Chart

```text
  High  |                                        Service Mesh
Benefit |                          CQRS *              *
        |               Saga *
        |     Circuit Breaker *
        |  Strangler Fig *
   Low  |
        +─────────────────────────────────────────────────────
              Low                Complexity                High
```

> **Rule of thumb:** Start with Circuit Breaker (lowest cost, immediate resilience benefit). Add Saga when you have multi-service transactions. Add CQRS when read/write load diverges. Add Service Mesh when your cluster grows beyond ~10 services. Use Strangler Fig only when you have an existing monolith to migrate.
