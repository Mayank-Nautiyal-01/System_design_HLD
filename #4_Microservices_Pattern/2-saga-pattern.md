# The Saga Pattern-[important]

## The Distributed Transaction Problem

In a monolith, placing an order was a single, atomic database transaction:
```sql
BEGIN TRANSACTION;
  INSERT INTO orders ...
  UPDATE inventory SET stock = stock - 1 ...
  INSERT INTO payments ...
COMMIT;  -- all succeed, or all are rolled back. Guaranteed.
```

In a microservices world, these three operations live in **three completely separate services**, each owning its own private database. There is no single database transaction spanning all three. If the Payment Service fails after the Order Service and Inventory Service have already committed their changes, you have **inconsistent data** — an order was created, stock was decremented, but no payment was charged.

This is the **Distributed Transaction Problem**, and the **Saga Pattern** is the industry-standard solution.

## What Is a Saga?

A Saga is a sequence of **local transactions**, where each step publishes an event or a command that triggers the next step. Critically, if **any step fails**, the Saga executes a series of **compensating transactions** — explicit undo operations — to roll back the already-completed preceding steps.

There is no global lock, no two-phase commit. Just a chain of local commits and a defined rollback procedure.

## The Two Implementations of Saga

### 1. Choreography-Based Saga (Event-Driven)
Services react to each other's events autonomously. There is **no central coordinator** — each service knows what event to publish after completing its local transaction.

```text
  Order Service          Inventory Service        Payment Service
       |                        |                        |
  (1) Create Order              |                        |
       |──── "OrderCreated" event ────────────────────>  |
       |                 (2) Reserve Stock               |
       |                        |──── "StockReserved" event ──>
       |                        |                 (3) Charge Card
       |                        |                        |──── "PaymentSuccess" event ──> [Done]
       |                        |
       |                 [If Stock fails]
       |                        |──── "StockFailed" event ──────────>
       |                   Inventory                  [All listeners
       |                  publishes                    cancel upstream]
       |                 compensating
       |                   events
```

**Pros:** Simple, loosely coupled — each service only knows about its own events. No single point of failure.

**Cons:** Very hard to track the overall state of a business transaction (which step is it on?). Debugging a failed saga across event logs is extremely painful. Logic becomes scattered across services.

---

### 2. Orchestration-Based Saga (Central Coordinator)
A dedicated **Saga Orchestrator** service explicitly tells each participant service what to do, step by step, via direct commands. The orchestrator is the single source of truth for the transaction's state.

```text
                      [ Saga Orchestrator ]
                              |
     ┌────────────────────────┼────────────────────────┐
     |                        |                        |
(1) "CreateOrder"    (2) "ReserveStock"      (3) "ChargePayment"
     |                        |                        |
 Order Service          Inventory Service        Payment Service
     |                        |                        |
  "OrderCreated"         "StockReserved"          "PaymentFailed"
     └────────────────────────┴────────────────────────┘
                              |
                     [Orchestrator receives "PaymentFailed"]
                              |
                     Orchestrator sends compensating commands:
                     (4) "ReleaseStock"  ──> Inventory Service
                     (5) "CancelOrder"   ──> Order Service
```

**Pros:** Transaction state is centralized and fully visible. Easy to add/modify steps. Audit logs live in one place. Much easier to debug.

**Cons:** The Orchestrator is a new service that can become a bottleneck and a single point of failure if not designed carefully.

---

## Choreography vs. Orchestration: When to Use Which

| Factor                    | Choreography              | Orchestration                      |
|---------------------------|---------------------------|-------------------------------------|
| **Complexity**            | Low (few steps)           | Medium-High (many steps)            |
| **Coupling**              | Loosely coupled           | Services coupled to orchestrator    |
| **State visibility**      | Fragmented across logs    | Centralized in orchestrator         |
| **Debugging difficulty**  | High (trace events)       | Low (query orchestrator state)      |
| **Best for**              | Simple, 2-3 step flows    | Complex, multi-step business flows  |

**Rule of thumb:** Use **Choreography** for simple, stable flows (2-3 services). Use **Orchestration** for complex, multi-step business transactions where observability and control are critical.

*(Popular Orchestration Tools: Netflix Conductor, Apache Airflow, AWS Step Functions, Temporal.io.)*

---

## Full System Architecture

### ❌ WITHOUT Saga — The Problem (Inconsistent Data!)

```text
  A user places an order. Three services each save to their OWN separate database.

  Step 1: Order Service saves the order ✅
  ┌────────────────┐  save order  ┌───────────────┐
  │  Order Service │ ────────────►│  orders DB    │ ← order row created
  └────────────────┘              └───────────────┘

  Step 2: Inventory Service reduces stock ✅
  ┌────────────────────┐  reduce stock  ┌───────────────────┐
  │ Inventory Service  │ ──────────────►│  inventory DB     │ ← stock = stock - 1
  └────────────────────┘                └───────────────────┘

  Step 3: Payment Service tries to charge the card ❌ FAILS
  ┌────────────────────┐  charge card  ┌───────────────────┐
  │  Payment Service   │ ─────────────►│  payments DB      │ ← ❌ Card declined!
  └────────────────────┘               └───────────────────┘

  RESULT: Order exists ✅, Stock was reduced ✅, but Payment failed ❌
  The data is now WRONG across all three services. Nobody fixes it automatically!
```

---

### ✅ WITH Saga (Orchestration style) — The Solution

```text
  A "Coordinator" (the Saga) is in charge of the whole process.
  If any step fails, the Coordinator tells the previous steps to UNDO themselves.

  ┌───────────────────────────────────────┐
  │         SAGA COORDINATOR              │  ← the manager who tracks every step
  │  (knows the full order of steps)      │
  └───────────────────────────────────────┘
           │
           │  HAPPY PATH — everything works fine:
           │
           ├─── Step 1 ───► Order Service      ──► "Order created"    ✅
           ├─── Step 2 ───► Inventory Service  ──► "Stock reduced"    ✅
           ├─── Step 3 ───► Payment Service    ──► "Card charged"     ✅
           └─── Step 4 ───► Delivery Service   ──► "Driver assigned"  ✅
                                                    → ORDER COMPLETE 🎉

  ─────────────────────────────────────────────────────────────────────
           │
           │  FAILURE PATH — payment fails at Step 3:
           │
           ├─── Step 1 ───► Order Service      ──► "Order created"    ✅
           ├─── Step 2 ───► Inventory Service  ──► "Stock reduced"    ✅
           ├─── Step 3 ───► Payment Service    ──► ❌ Card declined!
           │
           │  Coordinator sees the failure, sends UNDO instructions in reverse:
           │
           ├─── UNDO Step 2 ───► Inventory  ──► "Put the stock back"   ✅
           └─── UNDO Step 1 ───► Order      ──► "Cancel the order"     ✅
                                                → Everything is clean again 🔄
```

![Saga Pattern Architecture](../../assets/architecture/saga_pattern_architecture.png)

**The big idea:** The Coordinator keeps a record of every step. If something breaks halfway, it knows exactly which steps to undo — like Ctrl+Z for your entire business process.

---

### Common HLD Interview Questions

**Q1: Why can't microservices simply use a traditional distributed 2-Phase Commit (2PC) protocol instead of the Saga Pattern?**
*Answer:* 2-Phase Commit requires a **distributed lock** across all participating databases for the entire duration of the transaction. In a microservices architecture with dozens of services, this creates catastrophic bottlenecks — all involved databases are locked and cannot serve other requests. Furthermore, 2PC has notoriously poor fault tolerance: if the coordinator crashes between Phase 1 and Phase 2, participating databases remain locked indefinitely until the coordinator recovers. Sagas avoid all of this by using short, independent local transactions with compensating rollbacks, requiring no distributed locks at all.

**Q2: Design the Saga flow for a food delivery app where a user places an order — cover both the happy path and a failure scenario.**
*Answer (Orchestration-based):*
- **Happy Path:** Orchestrator → (1) Order Service: `CreateOrder` → (2) Restaurant Service: `AcceptOrder` → (3) Payment Service: `ChargeCard` → (4) Delivery Service: `AssignDriver` → Order Complete.
- **Failure Scenario (Card declined at step 3):** Payment Service returns `PaymentFailed`. Orchestrator triggers compensating transactions: → (3b) Restaurant Service: `CancelOrder` → (2b) Order Service: `CancelOrder`. The compensating steps undo the prior committed steps in reverse order, maintaining data consistency across all services without a single distributed lock.
