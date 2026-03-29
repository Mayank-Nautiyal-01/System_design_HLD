# Stage 1: The Single Server Setup (Day Zero)

Every massively-scaled system in the world started exactly the same way — **one machine doing everything**. Understanding this starting point is critical because every architectural decision from here on is a *reaction* to a specific pain point that emerges as users grow.

---

### What Is the Single Server Setup?

In the single server setup, **one physical or virtual machine** handles every concern of your application:

- Serving the web pages / API responses (the Web/App Layer)
- Running the business logic (user login, payments, product search)
- Storing all the data (the Database)
- Serving static files (images, CSS, JavaScript bundles)

Everything lives together on a single box.

---

### Architecture Visualized

```text
          [ User's Browser / Mobile App ]
                        |
             (sends HTTP/HTTPS request)
                        |
                        v
     +------------------------------------------+
     |          Single Server (1 VM)            |
     |                                          |
     |   [ Web Server: Nginx / Apache ]         |
     |   [ App Logic:  Node.js / Django ]       |
     |   [ Database:   MySQL / SQLite ]         |
     |   [ Static Files: images, CSS, JS ]      |
     |                                          |
     +------------------------------------------+
```

![Single Server Setup Architecture](../../assets/architecture/single_server_setup.png)

**Real-World Example:** You build a food ordering app called *QuickBite*. You deploy it on a single $5/month DigitalOcean Droplet (1 CPU, 1GB RAM). Your app server (Express.js), database (SQLite), and all your food images are on that single machine.

---

### How a Request Flows Through This Setup

1. User opens `quickbite.com/menu` in their browser.
2. Their browser does a **DNS lookup** → gets the IP address of your single server.
3. Browser sends an HTTP GET request to that IP.
4. **Nginx** (Web Server) receives the request.
5. Nginx passes it to your **Node.js app** (App Logic).
6. Node.js queries the **SQLite database** for menu items.
7. Node.js builds the JSON response and sends it back via Nginx.
8. The browser renders the menu.

Total round trip: could be done in **~50ms**. Everything is on one machine, zero network hops between components.

---

## Advantages

1. **Simple to Develop:** A single `git push` + `pm2 start` is all you need to deploy. No complex networking between microservices.
2. **Performance:** The app talks to the DB over a local socket (nanoseconds) with zero network latency.
3. **Cost-Effective:** One $5–$10/month VPS is all you pay at the start.
4. **Simple Operations:** All logs are in one place: `tail -f /var/log/app.log`.
5. **Simplicity:** No distributed systems complexity like race conditions, split-brain, or network partitions.

---

## Disadvantages

1. **Single Point of Failure (SPOF):** If the disk fails or the power cuts out, your entire business is down. There is zero redundancy.
2. **Inflexible Scaling:** You cannot scale CPU-heavy components independently. You must scale the *whole* machine even if only one part needs it.
3. **Resource Competition:** Your app logic and database "fight" over the same CPU and RAM. A slow query can starve your web server.
4. **Security Risk:** Your database is exposed on the same public IP as your web server, increasing the attack surface.
5. **Hard Ceiling:** A single machine's specs have a physical limit (RAM/CPU). You cannot grow beyond one big box.

---

### Common HLD Interview Questions

**Q1: Why do companies start with a single server even though it's not scalable?**
*Answer:* Speed-to-market. The #1 killer of startups is not technical debt — it's running out of money before validating the product. A single server setup can be live in minutes and costs almost nothing.
*Example:* A founder building a new "Dog Walker" app uses a single AWS EC2 instance to prove people will actually use the service before spending months building a complex microservices cluster.

**Q2: What is a "Single Point of Failure" (SPOF) and why is it dangerous in production?**
*Answer:* A SPOF is any single component whose failure brings the *entire* system down. In a single server setup, the server itself is the SPOF.
*Example:* If the SSD on the `QuickBite` server wears out and fails, the entire business goes offline immediately with no fallback, resulting in lost revenue and customer trust.

**Q3: Why does database traffic compete with application traffic on a single server?**
*Answer:* Both processes share the same OS-level CPU scheduler and memory pool. A heavy workload in one impacts the other.
*Example:* A daily "Midnight Sales Report" running on the database consumes 99% of the CPU, causing regular customers trying to place orders to experience 10-second delays or timeouts.
