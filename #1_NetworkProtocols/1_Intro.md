# Network Protocols: Introduction

## What is a network protocol?
A network protocol is a set of rules that defines how devices communicate over a network.

It specifies:
1. How data is formatted
2. How data is transmitted
3. How devices identify each other
4. How errors are detected and handled

Without protocols, computers cannot reliably exchange meaningful information.

Simple example:
When you open a website, multiple protocols work together to deliver the page to your browser.

## Why do network protocols exist?
A network includes many different devices (laptops, servers, routers, smartphones). Protocols ensure all devices follow common communication rules.

### Benefits

1. Interoperability
Different devices, operating systems, and software can communicate using common standards.

Example:
A Windows laptop, an Android phone, and a Linux server can all access the same website because they follow protocols such as HTTP and TCP.

2. Reliable communication
Reliable communication means data is delivered correctly, in order, and with recovery from loss.

Example:
TCP provides reliability by:
- Detecting lost packets
- Retransmitting missing data
- Preserving packet order

3. Scalable internet infrastructure
Scalability means protocols can support a rapidly growing number of devices.

Example:
The internet supports billions of devices. Protocols such as IP, DNS, and HTTP are designed to scale as networks grow.

## How network protocols work together (step by step)

Step 1: User sends a request
A user enters a URL in the browser (for example, `www.example.com`).
The browser prepares a request.

Step 2: DNS resolves the domain
DNS converts the domain name into an IP address.
Example: `www.example.com -> 93.184.216.34`

Step 3: Connection establishment
TCP establishes a connection between client and server so both sides can communicate.

Step 4: Secure communication (optional)
If the website uses HTTPS, TLS performs a handshake and sets up encryption.

Step 5: HTTP request is sent
The browser sends an HTTP request.
Example:
```http
GET /index.html HTTP/1.1
Host: example.com
```

Step 6: Server processes the request
The server receives the request and prepares a response (for example, an HTML page).

Step 7: Server sends response
The server sends an HTTP response (for example, `200 OK`) with content such as HTML, CSS, JavaScript, and images.

Step 8: Browser displays the page
The browser receives the data, reconstructs the page, and renders it.

## Protocol flow summary
- DNS: Finds server IP address
- TCP: Establishes reliable connection
- TLS: Secures communication (for HTTPS)
- HTTP: Transfers webpage data

---
### Common HLD Interview Questions

**Q1: Why are network protocols strictly necessary in distributed systems?**
*Answer:* Protocols provide standard rules for communication. Without them, devices running different OS or hardware wouldn't understand data handling. 
*Example:* When a mobile app (client) talks to an AWS server, they both use HTTP. Without HTTP rules, the server wouldn't know where the headers end and the body begins.

**Q2: How do standardized protocols aid in horizontal scaling?**
*Answer:* Standardized protocols decouple components, allowing interchangeable commodity servers. 
*Example:* A Load Balancer can blindly forward TCP traffic to 100 different backend servers without needing custom code for each server, as long as they all speak TCP.
