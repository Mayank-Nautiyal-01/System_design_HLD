# When to Use Which: The Migration Strategy

A common trap for junior engineers and new startups is to assume that because Netflix and Uber use Microservices, they should start their brand new application with Microservices.

In System Design, this is widely considered an architectural anti-pattern.

## The Monolith-First Doctrine
Martin Fowler famously popularized the **"Monolith-First"** approach. 

1. **Always start with a Monolith.**
2. When the application succeeds and the team size scales to the point where developers are constantly blocking each other with merge conflicts, **then** you begin extracting microservices.

### Why Start with a Monolith?
Startups are trying to find "Product-Market Fit." The structure of the business and the code will change dramatically every week. Monoliths allow you to rapidly delete, rewrite, and refactor business logic because everything is in one codebase. 
If you start with Microservices, refactoring a feature that spans across 4 different network services and repositories is a nightmare. Furthermore, the operational cost of managing Kubernetes and CI/CD pipelines for 10 microservices will drain a startup's time and money.

## When Do You Migrate to Microservices?
You migrate when the pain of the monolith outweighs the immense network overhead of microservices:
1. **Team Scaling Pain:** You have 50+ developers and deploying the monolith takes 3 hours because of test suites and QA bottlenecks.
2. **Resource Scaling Pain:** One specific feature heavily demands GPU or CPU, while the rest of the app demands RAM.
3. **Release Cycles:** Different teams need to release code at completely different speeds (e.g., Marketing needs to deploy daily, but Finance only deploys monthly).

## The "Strangler Fig" Migration Pattern
You do not rewrite a Monolith into Microservices in one weekend. You use the **Strangler Fig Pattern**.

In nature, a Strangler Fig vine wraps itself around a massive tree, slowly growing and taking over until the original tree dies underneath it.
In software:
1. You identify the most painful, isolated module in the monolith (e.g., the Image Resizer).
2. You build a new, separate "Image Service".
3. You route all new traffic to the new Image Service via an API gateway.
4. You delete the Image Resizer code from the Monolith.
5. You repeat this for module after module (strangling the monolith) over 2 years until the monolith is completely gone.

---

### Common HLD Interview Questions

**Q1: Why should 99% of new applications and startups begin with a Monolithic architecture rather than jumping straight to Microservices?**
*Answer:* Startups need extreme agility to pivot business logic quickly. Microservices establish rigid, hard-to-change network boundaries. Refactoring code across multiple bounded contexts is exponentially harder than dragging and dropping folders in a solitary Monolith.
*Example:* A startup realizes their "User" model actually needs to be completely merged into their "Company" model. In a single Node.js monolith, hitting "Find and Replace" takes 5 minutes. In a microservices architecture, this requires deprecating APIs, coordinating deployments across two teams, and migrating data across physical network partitions.

**Q2: Describe the "Strangler Fig" migration pattern and why it is superior to a "Big Bang Rewrite".**
*Answer:* The Strangler Fig pattern involves incrementally extracting one feature at a time from a monolith into a new microservice, routing traffic to it, and deleting the legacy code. It is superior because it delivers immediate value while drastically minimizing risk.
*Example:* If a company tries a "Big Bang Rewrite" (freezing all new features for 2 years to rebuild everything perfectly in microservices from scratch in a darker room), they will usually run out of money or the market will outpace them. Strangler allows the company to keep shipping features *while* upgrading the architecture.
