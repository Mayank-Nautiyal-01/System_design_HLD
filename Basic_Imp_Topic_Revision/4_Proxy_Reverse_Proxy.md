# Proxy vs Reverse Proxy

**Topic**: Proxy and Reverse Proxy

## Yeh kya hota hai? (What is it?)
Proxy ka matlab hota hai ek 'Middleman' ya bicholiya. Ye aesa server hota hai jo client (aap) aur main server (jahan data hai) ke beech mein khada hota hai. Iske do main types hote hain: **Forward Proxy (ya bas Proxy)** aur **Reverse Proxy**.

### 1. Forward Proxy (Client ka helper/guard)
Ye client ki taraf se kaam karta hai. Client jo bhi request bhejta hai, pehle proxy server ke paas jaati hai, fir proxy request aage main server ko bhejta hai.
* **Why use it?** Anonymity ke liye (taaki main server ko client ka IP na pata chale), ya school/office mein kuch websites ko block karne ke liye, ya static content jaldi load karne ke liye (Cahce).
* **Example**: Aap office/college network se YouTube kholna chahte ho lekin unhone Firewall/Proxy laga rakha hai jo media sites ko block karta hai. Proxy decide karta hai ki request bahar jayegi ya nahi.

### 2. Reverse Proxy (Server ka helper/guard)
Ye server ki taraf se kaam karta hai. Jab client internet se request bhejta hai, toh woh pehle ek server se takrati hai (Reverse Proxy), aur fir ye decide karta hai ki isse peeche baithe konse real backend server par bhejna hai.
* **Why use it?** Load Balancing (bahut saari requests aayin toh unko alag-alag servers mei baat dena), Security (taaki real server ka IP kisi ko pata na chale, DDoS attacks rokna), SSL Termination.
* **Example**: Jab aap `amazon.in` visit karte hain, to aap sidhe unke us database server se connect nahi hote jahan aapka data rakha hai. Aapki pehli mulakat ek Reverse Proxy (jaise Nginx, HAProxy) se hoti hai, jo dekhta hai ki server-1 free hai ya server-2, aur us hisab se aapki request backend me forward karta hai. 

## Key Difference:
* **Forward Proxy**: Protects clients (Hides the client from the server).
* **Reverse Proxy**: Protects servers (Hides the server from the clients).
