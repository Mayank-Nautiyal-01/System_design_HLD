# Microservices Architecture

**Topic**: Microservices 

## Yeh kya hota hai? (What is it?)
Pehle ke time mein ek app ka saara code (UI, Database connection, Payment logic, User management) ek hi bade code-base project mein rakha jata tha. Use "**Monolithic Architecture**" kehte hain. Jab project bahut bada ho jata tha, tab choti si bug-fix deploy karne k liye poori app build & deploy karni padti thi.

**Microservices** ek naye tarike ki design approach hai. isme us poore bade project ko chhote-chhote **Independent Services** mein todd diya jata hai. Har chhota service ek single business capability ko handle karta hai (Jaise sirf Payment, ya sirf Email).

Yeh services apas mein ek dusre se lightweight protocols (jaise HTTP/REST ya gRPC) ke through APIs ka use karke baat karte hain.

## Example
Ek badi E-commerce site (Amazon) ko dekho, ye monolithic nahi ho sakti, ishe **Microservices** se design karte hain:
1. **User Service**: Jo sirf Login/Signup ka kaam dekhegi.
2. **Catalog Service**: Jo sirf Products ki detail & images dekhegi (Database bhi alag).
3. **Cart Service**: Jo user ke Add to Cart item ko Redis/kisi other memory me rakhogi.
4. **Payment Service**: Jo sirf payment gateway API se checkout handle karegi.
5. **Notification Service**: Jo email ya SMS bhejegi order hone par.

Ye 5 lag-alag Microservices hain (Apne apne alag database ke sath/ alag langugaes/ tech stack- Python, NodeJS, Java). Sab independent team sambhalti hai. Jisse update push karna "Agile" aur super fast ho jata hai. Agar "Payment Service" aaj server-down/crash hogyi. Tto user ko E-commerce Catalog zaroor dikhega bas wo cart me pay nah kar pyega... Par site completely offline 'Down' nahi hogi. 

## Pros & Cons
**Fayde (Pros)**:
* Har service ko alag se Horizonally/Vertically **Scale** kiya ja sakta hai (Bina dusri ki processing power laye).
* Koi service fail hone par pura app crash nahi hota. (Fault Isolation & resilience).

**Nuksan (Cons)**:
* System ko operate karna bahut mushkil hota hai. 
* Network calls ke wjhse Latency bdjati hai local method call k muqable.
