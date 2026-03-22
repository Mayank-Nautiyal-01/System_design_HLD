😒😒 Network protocols ==
 
🌟 so what is network protocols?
➡️ A network protocols is a set of rules that define how devices communicate with each other over a network.
it specifies:
 i> how data is formatted
 ii> how data is transmitted
 iii> how devices identify each other
 iv> how errors are handled

 Without protocols, computers would not understand how to exchange information.

 -simple ex:When you open a website, several protocols work together to deliver the webpage to your browser.

🌟 now why does network protocols exist?
➡️ so networks contains many diff devices like laptop,servers,routers,smartphones
   & protocols ensure that all devices follow standard comm. rules


  ⭐⭐ benefits::

 1> Interoperability
(ability of different devices, operating systems, or software systems to communicate and work together using common protocols)

ex: A Windows laptop, an Android phone, and a Linux server can all access the same website because they follow the same protocols like HTTP and TCP.

2>Reliable Communication
(the ability of a protocol to ensure that data is delivered correctly, in the right order, and without loss)

ex: The TCP protocol guarantees reliable communication by:
-checking for lost packets
-retransmitting missing data
-ensuring packets arrive in the correct order

3>Scalable Internet Infrastructure
(the ability of internet systems and protocols to support a very large and continuously growing number of devices without breaking)

Ex:
The internet currently supports billions of devices such as:phones,laptops,servers etc
- Protocols like IP, DNS, and HTTP are designed to scale so the network can keep growing.

🐾 Interview perspective: they may ask u how network protocols work 
- so lets see step by step

Step 1 - User Sends a Request:
A user enters a website URL in the browser (for example: www.example.com).
The browser prepares a request to retrieve the webpage.

step2 - DNS Resolves the Domain:
The DNS protocol converts the domain name into an IP address.
Example:
www.example.com → 93.184.216.34
This allows the computer to locate the server on the internet.

step3 - connection establishment
The TCP protocol establishes a connection between the client (user device) and the server by with both sides are ready to communicate.

step 4- secure communication(optional):
If the website uses HTTPS, the TLS protocol performs a handshake to establish encrypted communication.

Encryption ensures that the data cannot be read by attackers during transmission.

step 5- HTTP Request Sent

The browser sends an HTTP request to the server.
Ex request:
GET /index.html HTTP/1.1
Host: example.com
This asks the server for a specific resource.

step 6-Server Processes the Request

The server receives the request, processes it, and prepares the response (for example: an HTML webpage).

step 7- server sends response
The server sends an HTTP response back to the browser
response: 200 OK
The response may contain:html,css,js,images etc

Step 8 — Browser Displays the Page:
The browser receives the data, reconstructs the webpage, and displays it to the user.

➡️➡️so here u see how multiple protocols collaborate to complete a single internet request:

DNS → finds the server
TCP → establishes connection
TLS → secures communication
HTTP → transfers webpage dataa
