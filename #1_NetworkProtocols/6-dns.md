DNS (Domain Name System)

DNS is the system that converts human-readable domain names into IP addresses.

Computers communicate using IP addresses, not domain names.

Example:

youtube.com → 142.250.x.x

DNS performs this translation.

? Why DNS Exists

Humans remember names easily.

youtube.com
google.com
amazon.com

But computers identify servers using IP addresses.

Example:

142.250.182.46

DNS acts like the internet’s phonebook.

Domain Name → IP Address

-------------
Example: Opening a Website

You open YouTube in Google Chrome.

-Step 1 — Browser checks cache

First, the browser checks if it already knows the IP.

Browser cache
OS cache

If not found, DNS query is sent.

-Step 2 — DNS resolver query

Your computer asks a DNS resolver:

What is the IP of youtube.com?

-Step 3 — DNS server responds

DNS returns the IP address.

youtube.com → 142.250.x.x

Now the browser can connect to the server.

--- DNS Resolution Process:

When the resolver does not know the answer, it queries multiple DNS servers.

Client
 ↓
Recursive Resolver
 ↓
Root DNS Server
 ↓
TLD Server (.com)
 ↓
Authoritative DNS Server
 ↓
IP address returned

----------------------
DNS Uses UDP (Mostly)

DNS queries usually use:

--UDP

Why?

Because DNS responses are small and fast.

For large responses or zone transfers, it may use:

--TCP
------------------------
?? DNS Caching

DNS results are cached to reduce latency.

Caches exist at:

Browser
Operating System
DNS Resolver

Example:

youtube.com → cached IP

This avoids repeated DNS lookups.
------------
[OK]
DNS translates domain names into IP addresses, allowing users to access internet services using human-readable names instead of numerical IP addresses.

---
### Common HLD Interview Questions

**Q1: What happens if an upstream DNS resolver goes down?**
*Answer:* DNS heavily relies on multiple layers of caching.
*Example:* If your ISP's DNS is down, your local OS cache or browser cache will still remember that `youtube.com` maps to `142.250.x.x` from your visit 5 minutes ago.

**Q2: How can DNS be used to load balance incoming traffic in a global system architecture?**
*Answer:* Through Geo-DNS or Round-Robin DNS.
*Example:* If a user in India types `netflix.com`, Geo-DNS returns the IP of the Mumbai data center, whereas a user in the US gets the IP of the Virginia data center, reducing worldwide latency.