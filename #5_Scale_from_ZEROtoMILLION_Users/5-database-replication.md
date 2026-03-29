# Stage 5: Database Replication

Your database server is now the bottleneck. You have one powerful DB server handling *all* reads AND all writes. As your user base grows to hundreds of thousands, this single DB becomes the chokepoint your engineers lose sleep over. The solution is **Database Replication**.

---

### What Is Database Replication?

Database replication is the process of **automatically copying data from one database server (the Primary) to one or more other database servers (the Replicas/Secondaries)** in near-real-time.

The core insight that makes this powerful: **in most web applications, reads vastly outnumber writes.**

> **Typical Read/Write Ratio:** On a social media app like Twitter, for every 1 tweet written (write), there are ~1000 users viewing their feed (reads). This is a 1:1000 write-to-read ratio.

If reads dominate, you can massively increase read capacity by adding Read Replicas, without touching the write path at all.

---

### Master-Slave Replication Visualized

```text
         [ App Server Fleet ]
          /       |        \
         /        |         \
        v         v          v
 +----------+ +----------+ +----------+
 |  Write   | |  READ    | |  READ    |
 | traffic  | | traffic  | | traffic  |
 +----------+ +----------+ +----------+
        \         |          /
         \        |         /
          v       v        v
     +--------------------+  ← ALL WRITES go here
     |  PRIMARY (Master)  |
     |    Database        |
     +--------------------+
              |
       (Async replication stream)
       (Binary log / WAL)
              |
     +--------+--------+
     v                 v
+----------+      +----------+
|  READ    |      |  READ    |    ← ALL READS go here
| REPLICA  |      | REPLICA  |
| (Slave 1)|      | (Slave 2)|
+----------+      +----------+
```

![Database Replication Architecture](../../assets/architecture/database_replication.png)

### How the Replication Stream Works
1. App writes (`INSERT`, `UPDATE`, `DELETE`) go to the **Primary** only.
2. The Primary logs every change to its **Binary Log** (MySQL) or **WAL — Write-Ahead Log** (PostgreSQL).
3. Each Replica continuously reads this log from the Primary.
4. Replicas apply the same changes to their own copy of the data.
5. Within milliseconds, all Replicas have the same data as the Primary.

---

### Types of Replication

#### Synchronous Replication
The Primary waits for **at least one Replica to confirm** it has written the data before acknowledging the write to the application.

```text
App → Write to Primary → Primary writes → Replica confirms → App gets "success"
```

**[YES] Zero data loss** on Primary failure (at least one Replica has the data).
**[NO] Higher write latency** — every write waits for a network round-trip to the Replica.

#### Asynchronous Replication (Default)
The Primary writes data and **immediately acknowledges** the application. The replication to Replicas happens in the background.

```text
App → Write to Primary → App gets "success" → [background] → Replica gets data
```

**[YES] Lower write latency** — app doesn't wait for Replicas.
**[NO] Replication lag** — Replicas may be slightly behind (milliseconds to seconds). If Primary crashes before replication completes, those writes are lost.

---

### Replication Lag: A Critical Concept

**Replication lag** is the delay between a write hitting the Primary and that write being visible on the Replicas.

**Problem Scenario — Read-Your-Own-Write Inconsistency:**
```text
1. Alice updates her profile picture.     → Write goes to Primary [OK]
2. Alice is immediately redirected to her profile page.
3. The page-load does a READ from Replica #1.
4. Replica #1 hasn't gotten the update yet (50ms lag).
5. Alice sees her OLD profile picture.
```

**Solutions to Replication Lag Issues:**
- **Read-After-Write Consistency:** For 1 second after a write, route that user's reads to the Primary.
- **Monotonic Reads:** Ensure a user always reads from the same Replica (via IP hash or user ID hash).
- **Synchronous Replication:** For critical paths (e.g., banking), accept the latency to guarantee consistency.

---

### Failover (When the Primary Dies)

Primary failure = total write outage. Automatic failover handles this:

1. Primary crashes → Health check detects failure.
2. Replica with most up-to-date data is PROMOTED to new Primary.
3. Load balancer/DNS is updated to point to the new Primary.
4. Application resumes writing seamlessly.

---

## Advantages

1. **Massive Read Scalability:** Add read replicas to handle 10x or 100x more traffic.
2. **High Availability:** If the Primary fails, a Replica takes over immediately.
3. **Geographic Distribution:** Place Replicas in different world regions for low-latency reads.
4. **Operational Safety:** Run heavy analytics or backups on a Replica without slowing down the Primary.

---

## Disadvantages

1. **Replication Lag:** Users might see stale data for a few milliseconds (eventual consistency).
2. **Write Bottleneck Persists:** Replication only scales reads. ALL writes still hit one Primary.
3. **Failover Complexity:** Automatic failover requires precise tooling to avoid "split-brain" issues.
4. **Storage Cost:** Each Replica is a full copy. 3 Replicas = 3x the storage bill.

---

### Common HLD Interview Questions

**Q1: Why does database replication primarily solve read scalability and not write scalability?**
*Answer:* In master-slave replication, all writes must hit the single Primary to maintain data integrity. Replicas are copies used only for reads.
*Example:* A "Social Media" app scales by adding 50 read replicas to handle millions of users viewing feeds, but all new "Likes" and "Comments" still converge on one Primary server.

**Q2: What is "split-brain" in database replication and how do you prevent it?**
*Answer:* It's when two nodes both believe they are the Primary after a network failure, causing data divergence. Prevent it using Quorum/Voting.
*Example:* A 3-node cluster uses a "Majority Vote." If the Primary loses connection to the other two, those two will elect a new leader between themselves, and the isolated node will step down.

**Q3: You have a read/write ratio of 95:5. How do you architect your database tier?**
*Answer:* Set up 1 Primary for writes and multiple Read Replicas for reads. Use a cache (Redis) to further absorb read load.
*Example:* A "News Website" routes all article visits (95%) to a pool of read replicas, while only editor updates and user comments (5%) hit the primary database machine.
