# Sockets and Ports

When data travels across the internet, the **Network Layer (IP)** ensures the data reaches the correct *machine* (like a server or a laptop). However, a single machine might be running hundreds of different applications simultaneously (a web server, an email client, a database, etc.). 

How does the machine know which specific application should receive the incoming data? That is where **Ports** and **Sockets** at the Transport Layer come in.

---

## 1. What is a Port?
A port is a logical endpoint represented by a 16-bit number (ranging from `0` to `65535`). If an IP address is like the street address of a large apartment building, the Port is the specific apartment number inside that building.

Ports are divided into three standard ranges:

### Well-Known Ports (0 – 1023)
Reserved for fundamental system processes and standard protocols.
- **Port 80:** HTTP (Unencrypted Web)
- **Port 443:** HTTPS (Secure Web)
- **Port 22:** SSH (Secure Login)
- **Port 53:** DNS (Domain Name System)
- **Port 25:** SMTP (Sending Email)

### Registered Ports (1024 – 49151)
Used by specific applications and services, often registered with IANA.
- **Port 3306:** MySQL Database
- **Port 5432:** PostgreSQL
- **Port 6379:** Redis cache
- **Port 8080:** Common alternative for HTTP (e.g., Tomcat, local dev servers)

### Ephemeral / Dynamic Ports (49152 – 65535)
Used temporarily by *clients*. When your browser makes an outgoing request to a website, your OS assigns it a random port from this high range for the duration of the connection.

---

## 2. What is a Socket?
A socket is the absolute fundamental endpoint for two-way communication. It is purely defined as the combination of an **IP Address** and a **Port Number**.

*Socket format:* `IP Address : Port`

### Example
When you go to `https://google.com`, the actual connection established is between two sockets:
- **Server Socket:** `142.250.190.78:443` (Google's IP + the HTTPS port)
- **Client Socket:** `192.168.1.15:52431` (Your laptop's IP + a random ephemeral port)

Data flows continuously back and forth between these two specific sockets.

---

### Common HLD Interview Questions

**Q1: Can two different applications listen on the exact same port on the same server simultaneously?**
*Answer:* No. Only one process can bind to a specific IP and Port combination at any given time.
*Example:* If you have an Apache web server currently running and listening for traffic on Port 80, attempting to start an Nginx server on Port 80 will result in a "Port already in use" (Address already in use) backend error.

**Q2: If a server only has one Port 443 for HTTPS, how is it capable of handling 50,000 concurrent user connections at the same time?**
*Answer:* A TCP connection is not identified solely by the server's port. It is uniquely identified by a "4-Tuple" consisting of: `(Source IP, Source Port, Destination IP, Destination Port)`.
*Example:* Even though 50,000 clients are all connecting to the server's identical `Destination Port 443`, every single client has a totally different `Source IP` or a randomly generated `Source Port` (like 51000, 51001, etc.). Because the 4-Tuple is entirely unique for every single user, the server can effortlessly distinguish and manage all 50,000 connections simultaneously.
