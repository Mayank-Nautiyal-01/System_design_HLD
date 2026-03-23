# Real-Time Protocols

Standard HTTP is based on a rigid **Request-Response** model: a client must explicitly ask the server for data, and the server cannot send anything until it is asked.

However, modern systems require **real-time** data delivery (like live chat, live sports scores, and multiplayer gaming), where the server needs to instantly push data to the client. Using traditional HTTP "polling" (asking the server "Are there new messages?" every 1 second) is incredibly wasteful for CPU and bandwidth.

To solve this, System Design relies on real-time protocols.

---

## 1. WebSockets
WebSockets provide a **persistent, full-duplex (two-way) communication channel** over a single TCP connection.

Once a WebSocket connection is established, the strict Request-Response rule is broken. Both the client and the server can stream data to each other at any exact moment.

### Key Characteristics:
- **Connection:** The client sends an initial HTTP request with an `Upgrade: websocket` header. If the server agrees, the connection is upgraded and left open continuously.
- **Direction:** Bi-directional (Client <-> Server).
- **Overhead:** Extremely low overhead per message compared to HTTP, because headers are not repeatedly sent.

*Example:* 
In a collaborative editing app like Google Docs, when you type a letter, your client uses WebSockets to instantly push the keystroke to the server. Simultaneously, the server uses the exact same WebSocket connection to instantly push your coworker's keystrokes back to your screen.

---

## 2. Server-Sent Events (SSE)
Server-Sent Events provide a **persistent, half-duplex (one-way) communication channel** strictly from the Server to the Client.

SSE uses standard HTTP natively. The client opens an HTTP connection, and the server holds it open and continuously streams a sequence of text events back to the client.

### Key Characteristics:
- **Connection:** Standard HTTP connection kept alive.
- **Direction:** Uni-directional (Server -> Client).
- **Benefits:** Unlike WebSockets, SSE natively supports HTTP features like automatic reconnections and multiplexing (over HTTP/2).

*Example:* 
In a live cricket scoreboard application, the user only needs to *receive* the score. The server uses SSE to push every run or wicket to the user's browser in real-time. The user's browser never needs to send dynamic data back.

---

## 3. Long Polling (The Fallback)
Before WebSockets and SSE, Long Polling was the primary workaround for real-time data.

The client sends a standard HTTP request to the server. Instead of responding immediately with empty data, the server **holds the request open** until new data is available. Once the data arrives, the server responds, closing the connection. The client instantly opens a brand new request to start waiting again.

*Example:* 
Waiting for an asynchronous video rendering job to finish. The client long-polls the server. The server holds the request open for 30 seconds. When the rendering finishes at the 15-second mark, the server sends the video link and the connection closes.

---

### Common HLD Interview Questions

**Q1: In a System Design interview, when would you choose Server-Sent Events (SSE) over WebSockets?**
*Answer:* You choose SSE when the data flow is strictly one-way (Server to Client) because SSE is structurally simpler and utilizes standard HTTP natively, whereas WebSockets require establishing and scaling a custom, dedicated bi-directional protocol.
*Example:* For a real-time stock price dashboard, the user merely watches the screen. The server continuously pushes price updates to the dashboard via SSE. The client never needs to push massive amounts of complex data back to the server.

**Q2: What is "Long Polling," and why is it considered inefficient compared to WebSockets for high-frequency chat applications?**
*Answer:* Long Polling involves holding an HTTP request open until the server has new data, then immediately opening a fresh request once the data is received. It forces the server to handle the intense overhead of constantly rebuilding HTTP connections.
*Example:* If two users are rapidly typing in a chat app (10 messages per second), Long Polling would require performing the TCP handshake to establish and tear down 10 unique HTTP connections per second, wasting CPU and bandwidth. WebSockets create *one* persistent TCP connection to stream all 10 messages instantly.
