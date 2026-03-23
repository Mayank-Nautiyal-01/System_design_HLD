# CP vs AP: Tradeoffs in Modern Systems

Since Partition Tolerance (P) is a mandatory requirement for any distributed system, HLD interviews will heavily evaluate your ability to correctly categorize and implement **CP Systems** vs **AP Systems**.

You must choose the database architecture that correctly aligns with your company's business goals.

## CP Systems (Consistency + Partition Tolerance)
A **CP Database** guarantees that every read gets the most recent write. If a network partition occurs and nodes cannot synchronize, the disconnected nodes will completely reject queries (terminate availability) to prevent serving stale data.

**Key Use Cases:**
- Banking and Financial ledgers.
- Digital Wallets or Inventory management.
- Airline booking systems.

**Popular CP Databases:**
1. **MongoDB:** A primary node handles writes. If the designated primary node is isolated via network partition, the remaining secondary nodes undergo a strict election process. During this election, the cluster sits entirely unavailable for writes. It favors C over A.
2. **HBase & Redis Cluster:** Strongly consistent distributed stores that halt operations if the cluster sync breaks down.
3. **CockroachDB / Spanner:** Distributed SQL systems utilizing strict consensus algorithms (Paxos/Raft) to prioritize perfect consistency globally.

## AP Systems (Availability + Partition Tolerance)
An **AP Database** guarantees that every query receives a rapid response. If a network partition occurs, the nodes simply reply to the user using the most recent data they locally possess, openly accepting that it might be heavily outdated. Once the network heals, the nodes resolve conflicts.

**Key Use Cases:**
- Social Media feeds (Facebook, Twitter).
- "Likes" or "Upvotes" counters.
- Product Catalog viewing or Reviews.

**Popular AP Databases:**
1. **Cassandra:** Built from the ground up to never go down. Masterless architecture allows clients to read and write to absolutely any node at any time, even if half the network is disconnected. Highly available, eventually consistent.
2. **DynamoDB (when configured for AP):** Amazon's key-value store defaults to eventual consistency, guaranteeing extreme availability.
3. **CouchDB:** Operates effectively even offline, syncing changes once connectivity is restored.

---

### Common HLD Interview Questions

**Q1: If you are designing the backend for an E-Commerce "Shopping Cart" service, would you choose a CP system or an AP system?**
*Answer:* You would surprisingly choose an **AP system**. In e-commerce, refusing to let a user add an item to their cart because a backend database replica is down directly loses revenue. It is vastly preferable to use an AP database like DynamoDB to accept every single cart addition regardless of state, and resolve conflicts later during the checkout phase.
*Example:* Amazon deliberately built DynamoDB specifically because their shopping carts were failing under CP systems. E-commerce platforms always prioritize "Always Writable" architectures.

**Q2: How does a consensus algorithm like "Raft" or "Paxos" relate to the CAP Theorem?**
*Answer:* Consensus algorithms are explicitly designed to maintain Consistency in a CP system. They mathematically ensure that a strict majority (quorum) of nodes must agree on a state change before telling the client "success."
*Example:* In a 5-node cluster running Raft, if a network partition splits 3 nodes into Group A and 2 nodes into Group B, Group A will maintain a quorum and accept writes (Consistency). Group B, realizing it lacks a majority, will actively shut down and reject queries (sacrificing Availability) to prevent a split-brain scenario.
