

# Transport Layer Protocols

The Transport Layer of the TCP/IP Model is responsible for      communication between applications running on different machines.

It ensures that data sent from one application reaches the correct application on another system.

Main responsibilities:

- Process-to-process communication
- Reliability
- Data ordering
- Flow control
- Port addressing

Two main protocols are used at this layer:

- TCP
- UDP

---

## 1. TCP (Transmission Control Protocol)

TCP is a connection-oriented and reliable protocol.

Before sending data, TCP establishes a connection between client and server.

==TCP Connection Setup

TCP uses a 3-way handshake.


Client → SYN
Server → SYN-ACK
Client → ACK


After this handshake, a connection is established.


+ Example: Opening a Website

You open YouTube in Google Chrome.

Steps:

1. Browser finds the IP using DNS
2. Browser connects to server using TCP


Client → connect to port 443


Port  443 is used by HTTPS     .

Then the browser sends an HTTP request.


GET / HTTP/1.1
Host: youtube.com
   

TCP ensures:

  - packets arrive
  - packets arrive in order
  - lost packets are resent

---

     TCP Reliability Example

Imagine sending data in  4 packets     .

   
Packet 1
Packet 2
Packet 3
Packet 4
   

If Packet 3 is lost , TCP detects the loss.

   
Server → missing packet
Client → resend Packet 3
   

This guarantees  accurate data delivery     .

---

     Where TCP Is Used

TCP is used where  reliability is important     .

Examples:

  - Web browsing
  - File downloads
  - Email systems
  - Database connections

Protocols using TCP include:

  - HTTP
  - HTTPS
  - SMTP

---

## 2. UDP (User Datagram Protocol)

UDP is a connectionless protocol     .

It does not establish a connection before sending data     .

There is:

  - no handshake
  - no retransmission
  - no packet ordering

Data is simply sent as datagrams     .

   
Client → Data → Server
   

---

     Example: Online Gaming

In an online game, players move constantly.

Example:

   
Player position updates
Frame 1
Frame 2
Frame 3
Frame 4
   

If Frame 2 is lost , it doesn't matter much because:

   
Frame 3 already has new position
   

Resending old data would create  lag     .

So games prefer UDP.

---

     Example: Video Calls

Video calls using WebRTC also use UDP.

   
Video Frame 1
Video Frame 2
Video Frame 3
   

If a frame is lost, the next frame continues.

Real-time speed matters more than reliability.

---

   TCP vs UDP

| Feature         | TCP                       | UDP               |
| --------------- | ------------------------- | ----------------- |
| Connection      | Connection-oriented       | Connectionless    |
| Reliability     | Guaranteed delivery       | No guarantee      |
| Packet ordering | Maintained                | Not guaranteed    |
| Speed           | Slower                    | Faster            |
| Use cases       | Web, email, file transfer | Streaming, gaming |

---

   Simple Analogy

     TCP     

   
Courier delivery
Package tracked
If lost → resend
   

     UDP     

   
Live radio broadcast
If signal drops → continue
   

---

  NOte:

When designing systems, engineers choose protocols based on      latency vs reliability tradeoff     .

Examples:

| System          | Protocol |
| --------------- | -------- |
| Web APIs        | TCP      |
| Video streaming | UDP      |
| Online gaming   | UDP      |
| File transfer   | TCP      |

---


> TCP provides reliable and ordered communication, while UDP provides faster communication with lower latency but without delivery guarantees.

---

### Common HLD Interview Questions

**Q1: When designing a real-time multiplayer shooting game, would you use TCP or UDP for transmitting player movement?**
*Answer:* You would use UDP because speed outweighs 100% reliability. 
*Example:* If a player's coordinates at `t=1s` are lost, TCP would wait to resend it, causing a freeze. With UDP, the game simply accepts the newer `t=1.1s` coordinate and ignores the lost packet.

**Q2: Explain why the TCP handshake requires 3 steps instead of just 2.**
*Answer:* The 3rd step proves to the server that the client is active. 
*Example:* Without the 3rd step, a delayed connection request from yesterday could reach the server today, forcing the server to open an unwanted connection and waste resources.
