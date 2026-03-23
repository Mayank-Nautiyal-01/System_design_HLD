# CAP Theorem: Consistency

In the CAP Theorem, **Consistency** is distinctly defined as *linearizable consistency*. 

This means that any read operation that begins after a write operation successfully completes must return that value, or the result of a later write operation. Put simply: **all nodes must see the exact same data at the exact same time.**

## Understanding Distributed the Consistency Problem

If you have a single database server running PostgreSQL, consistency is easy. There is only one source of truth.

However, in massive global systems (like Amazon), one server cannot handle 10 million simultaneous reads. To solve this, you create replica servers.
- User writes to **Node A** (Master).
- Node A must copy that data to **Node B** (Replica) and **Node C** (Replica).

### Strong Consistency
If a distributed database is configured for **Strong Consistency**, a write operation to Node A is strictly not considered "successful" until Node A has successfully forced the synchronization to Nodes B and C. 
- **Pros:** 100% data correctness. Users never see stale data.
- **Cons:** It takes significant time to wait for the network synchronization round-trips. If Node C is unplugged, the write to Node A will timeout or fail, meaning the database is effectively unavailable.

### Eventual Consistency
If a system sacrifices strong consistency, it often falls back to **Eventual Consistency**.
The write operation on Node A completes immediately, and the data is synchronized to B and C in the background asynchronously. 
- **Pros:** Extremely fast and highly available.
- **Cons:** A user might write their bio on Node A, instantly hit refresh, get routed to Node B (which hasn't synced yet), and confusingly see their old bio.

---

### Common HLD Interview Questions

**Q1: In an e-commerce platform, which subset of the system strictly requires Strong Consistency?**
*Answer:* The checkout, inventory management, and payment processing subsystems strictly require strong consistency to prevent overselling items or double-charging.
*Example:* If a warehouse has exactly one iPhone left, two users executing checkout simultaneously must not both "succeed." The database must serialize the writes and remain strictly consistent, locking out the second user immediately.

**Q2: Can a system be completely secure and robust if it only uses Eventual Consistency?**
*Answer:* Absolutely. Most major non-financial tech platforms operate on eventual consistency because the minor UX side-effect of "stale data" is heavily outweighed by the performance benefits of asynchronous replication.
*Example:* If a user adds a comment to a popular YouTube video, the internal database uses eventual consistency. It is completely acceptable if a user in Japan sees 5,000 comments while a user in Brazil only sees 4,990 comments for a few seconds. The data will eventually sync perfectly across all global nodes.
