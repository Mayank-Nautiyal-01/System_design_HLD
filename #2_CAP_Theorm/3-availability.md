# CAP Theorem: Availability

In the context of the CAP Theorem, **Availability** implies that every single non-failing node in a distributed system returns a rapid response for every read and write request, unconditionally. 

Crucially, an "Available" system will respond **without guaranteeing that it contains the most recent write.**

## The Goal of 100% Uptime

When a system is highly available (AP system), it prioritizes uptime over everything else, even data correctness.

Imagine a distributed database with nodes located in New York and London.
- A user updates their password on the New York node.
- Immediately after, a submarine cuts the transatlantic fiber-optic cable, completely severing network communication between New York and London.

If the system is designed to prioritize **Availability**:
When a European user tries to log in using the London node, the London node does not know if the user changed their password recently because it cannot talk to New York. The London node will simply utilize its local, potentially stale data (the old password) and allow the login. 

The system succeeded in remaining **Available**, but it willingly sacrificed **Consistency** to do so.

## Why Prioritize Availability?

For many massively scaling platforms, downtime translates directly into millions of dollars lost, angry users, and reputational damage. Refusing to serve a page load because one background replica database hit a temporary network snag is unacceptable for a global SaaS application.

---

### Common HLD Interview Questions

**Q1: How do modern systems implement high Availability natively at the database layer?**
*Answer:* They utilize horizontal scaling combined with completely decentralized replication architectures (like masterless clusters), where read and write queries can be served by multiple nodes concurrently.
*Example:* In Cassandra (an AP database), there is no single "Master" node. If an entire rack of servers catches fire (destroying multiple nodes), Cassandra seamlessly routes the client's query to surviving nodes holding replica data, meaning the user never experiences downtime.

**Q2: In a distributed system prioritizing Availability (AP), how do you resolve data conflicts when the network partition finally heals?**
*Answer:* Conflict resolution mechanisms are employed during the syncing process once the partition ends. This is typically handled by concepts like "Last Write Wins" (LWW) based on timestamps, or vector clocks.
*Example:* While London and New York were disconnected, user A bought item X in London, and user B bought item Y in New York. When the cable heals, DynamoDB will sync both ledgers and use precise internal Vector Clock timestamps to logically merge the shopping carts together.
