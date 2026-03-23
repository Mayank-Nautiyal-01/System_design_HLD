Communication model in network systems:
a comm. model defines how two or more systems exchanges data over a network.

In distributed systems and networking, the two most common models are:
i> client-server model
ii> peer-to-peer (p2p) model

1> client-server model: In the Client-Server model, communication happens between:

Client -> requests a service

Server -> provides the service

The server is usually a centralized system that listens for requests from multiple clients.

Client 1  ----\
Client 2  ----- >   Server
Client 3  ----/

Example: When you open a website:
- Your browser acts as a client
- The website backend acts as a server
- Client sends HTTP request
- Server responds with HTML / data

Browser (Client) -> HTTP Request -> Web Server
                                    v
Browser (Client) <- HTTP Response <---

Real-world examples: These protocols follow the Client-Server model
Web Browsing -> HTTP/HTTPS

Email -> SMTP / IMAP

Databases -> MySQL Server

APIs -> REST APIs

2> Peer-to-Peer (P2P) Model:

In Peer-to-Peer, every node acts as both:

Client

Server

There is no central server.

Each node communicates directly with other nodes.

Architecture:
Peer A  <->  Peer B
   |        |
Peer C  <->  Peer D

Every peer can:

- Request data

- Provide data

Example: File Sharing (BitTorrent)

A good example is BitTorrent.

Imagine a file split into 10 pieces.

Step 1 - Join the network

You open a torrent client.

Your PC -> discovers peers who have the file

Step 2 - Download pieces from multiple peers
Peer A -> piece 1
Peer B -> piece 2
Peer C -> piece 3
Peer D -> piece 4

You download different parts simultaneously.

Step 3 - You also upload to others

As soon as you download a piece:

You -> send piece 1 to another peer

Now you are also acting as a server.

------------------------------------------------------

Interview question: Why HTTP uses Client-Server but WebRTC uses P2P.
==> so in simple and short we can say -> HTTP uses the client-server model because web resources are centralized, while WebRTC uses peer-to-peer communication to enable low-latency real-time data transfer like video and voice calls.

---
### Common HLD Interview Questions

**Q1: What are the primary tradeoffs between Client-Server and Peer-to-Peer models for large-scale content delivery?**
*Answer:* Client-Server offers centralized control and simpler security but has scaling bottlenecks. P2P offers immense scalability and high fault tolerance but lacks governance.
*Example:* Netflix uses Client-Server to control DRM and billing centrally, while an open-source decentralized file network uses P2P to avoid server costs.

**Q2: If you were designing BitTorrent, why would you choose P2P over Client-Server?**
*Answer:* To eliminate server bandwidth bottlenecks. 
*Example:* Distributing a 50GB game update to 1 million users simultaneously would crash a central server. In P2P, those 1 million users download and upload chunks to each other, distributing the load.
