# Inter-Service Communication

In a monolith, components communicate instantly via in-memory function calls. In a microservices architecture, services must communicate identically over the physical network. 

This communication is broadly split into two distinct categories: **Synchronous** and **Asynchronous**.

## 1. Synchronous Communication (Request / Reply)
In synchronous communication, **Service A** calls **Service B** and strictly waits (blocks) until Service B sends a response back. The thread on Service A cannot continue until the network call finishes.

**Primary Protocols:**
- **HTTP / REST:** Simple, universally understood, easily testable via Postman using JSON.
- **gRPC:** Developed by Google. Uses Protocol Buffers (binary data) instead of JSON. It is massively faster, smaller in payload size, and excellent for strict internal service-to-service communication.

**The Danger (Cascading Failures):**
If Service A synchronously calls B, and B synchronously calls C... you have tightly coupled the architecture. If Service C goes down, B freezes waiting for C, and A freezes waiting for B. A single failure cascades up the chain and crashes multiple services.

## 2. Asynchronous Communication (Event-Driven / Messaging)
In asynchronous communication, **Service A** produces a message and drops it into a queue, and immediately moves on with its life. **Service B** will eventually read that message whenever it is ready.

**Primary Protocols:**
- **Message Brokers (RabbitMQ, AWS SQS):** Great for distributed task queues. 
- **Event Streaming (Apache Kafka):** Great for pub/sub architectures where 5 different services all need to react to a single event (like a "User Registered" event).

**The Benefit (Loose Coupling & Resiliency):**
If the "Email Service" crashes, the "Checkout Service" can still drop the "Send Receipt" message into the Kafka queue instantly and return HTTP 200 OK to the user. The message simply waits safely in the queue. 5 hours later, when DevOps restarts the Email Service, it wakes up, reads the queue, and sends the email without losing any data.

---

### Common HLD Interview Questions

**Q1: In an e-commerce platform, what is the fatal flaw of deeply nesting synchronous REST calls (e.g., API Gateway -> Checkout -> Inventory -> Payment)?**
*Answer:* Nested synchronous calls ruin both Latency and Availability. 
*Example:* If Checkout takes 50ms, Inventory takes 100ms, and Payment takes 200ms, the user has to stare at a loading spinner for a compounded 350ms. Furthermore, if the Payment service is temporarily restarting, the entire REST chain shatters and the user gets a 500 Server Error, aggressively destroying the company's checkout conversion rate.

**Q2: When would you strictly prefer a Synchronous gRPC call over an Asynchronous Kafka event payload?**
*Answer:* You use Synchronous gRPC when the calling service *absolutely cannot proceed* without knowing the immediate result of the downstream system. You use Asynchronous when "fire and forget" is acceptable.
*Example:* 
- **Synchronous:** Service A asks the Authorization Service "Does User ID 45 have admin rights?" Service A *must* block and wait for a strict Yes/No before rendering the admin dashboard. 
- **Asynchronous:** Service A tells the Notification Service "User 45 clicked a button, email them a thank you." Service A drops this in Kafka and instantly moves on, as it doesn't care exactly *when* the email gets sent.
