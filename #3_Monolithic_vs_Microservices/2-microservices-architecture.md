# Microservices Architecture

Unlike a monolith, a **Microservices Architecture** structures an application as a collection of small, autonomous, loosely coupled services. 

Each microservice implements exactly one specific business capability (e.g., a "Payment Service", a "Notification Service", a "User Management Service"). Each service runs in its own isolated process (often in its own Docker container) and strictly manages its own dedicated database.

### Architecture Visualized:
```text
           [ Mobile App / Browser ]
                      |
                      v
              [ API Gateway ]
              /       |       \
            /         |         \
          /           |           \
        v             v             v
[User Service]  [Cart Service]  [Payment Service]
      |               |                 |
      v               v                 v
 [(User DB)]      [(Cart DB)]      [(Payment DB)]
```

![Microservices Architecture](./assets/microservices_architecture.png)

## Advantages
1. **Independent Scaling:** If Black Friday traffic spikes, you can spin up 100 copies of the "Checkout Service" without touching the "Review Service." 
2. **Small Blast Radius:** If the "Notification Service" crashes due to a bug, it simply goes offline. The "Checkout Service" keeps running perfectly fine, so the company continues making money.
3. **Polyglot Programming:** Teams can choose the best tool for the job. The Payments team can write their service in secure, strongly-typed **Java**. The Machine Learning team can write their service in **Python**. The frontend API team can write their service in **Go** for speed.
4. **Independent Deployments:** A team of 5 developers can deploy an update to their specific service on a Tuesday afternoon without coordinating with the other 500 developers in the company.

## Disadvantages
1. **Devastating Network Complexity:** In a monolith, calling another function takes 1 nanosecond in memory. In microservices, calling another service means making an HTTP/Network call over the wire. The network can fail, timeout, or drop packets, introducing massive unreliability.
2. **Distributed Transactions:** If a user buys an item, you must charge their card (Payment Service) and remove the item from stock (Inventory Service). If the Payment succeeds but the Inventory network call fails, your data gets out of sync across multiple databases. This requires highly complex "Saga Patterns" to rollback distributed actions.
3. **Operational Overhead:** Instead of maintaining 1 server, the DevOps team must now monitor, log, trace, and secure 50+ different network services using Kubernetes, Datadog, Zipkin, etc.

---

### Common HLD Interview Questions

**Q1: What is the "Database per Service" pattern, and why is it strictly enforced in a true Microservices architecture?**
*Answer:* "Database per Service" means Service A and Service B absolutely cannot share the same database tables. If Service A needs data from Service B, it must ask Service B via a network API call. This prevents tight data coupling.
*Example:* If the "Order Service" could directly SELECT from the "User Service" database table, and the User team decided to rename a column, it would instantly break the Order Service database query. By forcing API communication, the User team can change their private database schema freely without breaking other teams.

**Q2: What is "Polyglot Programming" and why is it considered a major organizational advantage of Microservices?**
*Answer:* Polyglot Programming is the practice of using multiple different programming languages and databases within the same overall application ecosystem. 
*Example:* A monolith forces everyone to use Java. In microservices, the Search team can build their service using **Python and Elasticsearch**, while the highly concurrent Chat team builds their service using **Golang and WebSockets**. Each team uses the optimal tech stack for their specific domain without conflicting.
