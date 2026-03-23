# HTTP and HTTPS
=>Most communication on the web happens using:

HTTP

HTTPS

These protocols operate at the Application Layer of the TCP/IP Model.

They are used for communication between:

Client (browser / mobile app)
Server (web server / API server)


## 1. What is HTTP?

HTTP is a protocol used to transfer web resources between client and server.

Resources include:

HTML pages

Images

Videos

API data

Example request from Google Chrome:

GET /index.html HTTP/1.1
Host: example.com

The server processes the request and returns a response.

Example response:

HTTP/1.1 200 OK
Content-Type: text/html

Then the webpage content follows.

## 2. HTTP Request–Response Model

HTTP follows a request-response communication model.

Step 1 — Client sends request

Example:

GET /users HTTP/1.1
Host: api.example.com
Step 2 — Server processes request

Server may:

fetch data from database

execute backend logic

generate response

Step 3 — Server sends response

Example:

HTTP/1.1 200 OK
Content-Type: application/json

Response body:

{
  "name": "John",
  "age": 25
}
## 3. HTTP Methods

HTTP defines several methods that specify the action.

Method	Purpose
GET	Retrieve data
POST	Send new data
PUT	Update data
DELETE	Remove data

Example API:

GET /users
POST /users
DELETE /users/10

These are used in REST APIs.

## 4. What is HTTPS?

HTTPS is simply HTTP + encryption. It provides encryption, authentication, and data integrity

It uses:

TLS(Transport Layer Security)

HTTPS protects data by encrypting communication between client and server.

Example secure URL:

https://example.com
Why HTTPS Is Important

Without encryption, attackers could intercept data.

Example attack:

User → login request → server

If using HTTP:

password = visible

With HTTPS:

password = encrypted

## 5. HTTP vs HTTPS
Feature	        HTTP	HTTPS
Encryption	    No	    Yes
Security	    Low	    High
Default Port    80	    443
DataProtection	None	TLS encryption

## 6. Example: Opening a Website

You open YouTube.

Steps:

Browser resolves domain using DNS

Browser establishes TCP connection

TLS handshake happens

Browser sends HTTPS request

Example:

GET / HTTP/1.1
Host: youtube.com

Server responds with webpage data.

## 7. Why HTTP Is Stateless

HTTP is a stateless protocol.

This means:

Each request is independent

Example:

Request 1 → login
Request 2 → view profile

The server doesn't automatically remember previous requests.

To maintain user sessions, systems use:

cookies

session tokens

authentication headers



### In system design, HTTP is used for:=

-web applications
-REST APIs
-microservices communication

Typical architecture:

Client
  ↓
HTTP request
  ↓
Load Balancer
  ↓
Backend Service
  ↓
Database

[OK] HTTP is the fundamental protocol used for communication between clients and servers on the web, while HTTPS adds encryption using TLS to ensure secure communication.

---
### Common HLD Interview Questions

**Q1: HTTP is a "stateless" protocol. How do web applications maintain user sessions?**
*Answer:* Clients attach a token to every request so the server can re-identify them continuously.
*Example:* After a user logs in, the browser receives a JWT token cookie. Every subsequent request sends this cookie, so the server instantly knows who they are without storing a huge session table.

**Q2: Why is it crucial in System Design that HTTP `GET` and `PUT` requests be Idempotent, while `POST` is not?**
*Answer:* Idempotency guarantees safe retries if a network times out. 
*Example:* If a network glitch happens during a `PUT` request to update an email, retrying it 5 times simply updates the email 5 times. But retrying a `POST /checkout` 5 times could charge the user's credit card 5 times!