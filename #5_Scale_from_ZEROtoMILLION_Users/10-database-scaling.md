# Stage 10: Database Scaling (Sharding + NoSQL)

You've added DB Replicas to handle reads. But your *write* traffic has grown so massive that a single Primary database server is maxed out — disk I/O at 100%, write query queue backing up, latency spiking. This is the hardest scaling problem in all of systems design: **scaling writes**.

There are two major strategies: **Sharding** (horizontal partitioning) and **switching to NoSQL databases** that are purpose-built for horizontal write scalability.

---
### Why Writes Are Hard to Scale

- **Read Scaling:** Easy. Add Read Replicas.
- **Write Scaling:** Hard. All writes go to the Primary machine. You eventually hit a physical wall.

Splitting data across multiple machines is called **Sharding**.

---

### Database Sharding

Dividing your dataset into **smaller subsets called shards**. Each shard is a fully independent database.

```text
WITH SHARDING (3 shards split by user ID):

Shard 0 (userID % 3 == 0)    Shard 1 (userID % 3 == 1)    Shard 2 (userID % 3 == 2)
+-----------------------+    +-----------------------+    +-----------------------+
| User 3 | User 6 | ... |    | User 1 | User 4 | ... |    | User 2 | User 5 | ... |
+-----------------------+    +-----------------------+    +-----------------------+
```

---

### Sharding Strategies

1. **Range-Based:** Split by IDs (1-100, 101-200). Simple but causes "Hotspots" (new users always hit the latest shard).
2. **Hash-Based:** `hash(key) % total_shards`. Spreads data evenly but makes range queries hard.
3. **Consistent Hashing:** Minimizes data movement when adding shards. Used by Cassandra and DynamoDB.

---

### The Resharding Problem
Adding a new shard requires moving data. If you use simple modulo (`% 3` to `% 4`), up to 75% of your data might need to move, which is extremely dangerous for live systems.

---

### Problems With Sharding

- **Cross-shard JOINs:** Impossible without complex app-layer code.
- **Transactions:** Real ACID transactions across shards are slow and complex.
- **Celebrity/Hotspot:** One famous user can overwhelm their specific shard.

---

### NoSQL: Built for Horizontal Scale
When sharding SQL becomes too complex, NoSQL is the answer.

- **Key-Value (Redis/DynamoDB):** Perfect for sessions and profiles.
- **Document (MongoDB):** Flexible schemas for catalogs.
- **Wide-Column (Cassandra):** Extreme write-heavy scale. Netflix uses this for trillions of events.

---

## Advantages

1. **Infinite Write Scalability:** More shards = more write capacity.
2. **Failure Isolation:** One shard crashing only affects a fraction of users.
3. **Data Locality:** Store a French user's data on a shard in Europe.

---

## Disadvantages

1. **Extreme Complexity:** Sharding is a "last resort" because it breaks JOINs and transactions.
2. **Operational Overhead:** Managing 50 shards is much harder than managing 1 database.
3. **Resharding Pain:** Growing from 10 to 20 shards is a multi-day data migration project.

---

### Common HLD Interview Questions

**Q1: When should you start thinking about database sharding?**
*Answer:* Only as a last resort. Optimize queries, add indexes, vertically scale, and add read replicas first.
*Example:* A "Trading App" only shards its transaction database when the single largest AWS database instance reaches 100% CPU usage during peak market hours.

**Q2: What is a "hotspot shard" and how do you prevent it?**
*Answer:* When one shard gets 90% of traffic because of a popular key. Prevent using composite keys or salt.
*Example:* On "Instagram," Justin Bieber's user ID would be a hotspot. To fix this, you might split his data across multiple sub-shards using a `random_salt` + `user_id` as the sharding key.

**Q3: SQL vs NoSQL — how do you choose?**
*Answer:* Use SQL for structured data and ACID transactions. Use NoSQL for massive, unstructured write-heavy data.
*Example:* Use SQL (Postgres) for a "Banking App" to ensure balance transfers are perfectly consistent. Use NoSQL (Cassandra) for a "Smart Home Sensor" platform that receives 10 million temperature readings every second.
