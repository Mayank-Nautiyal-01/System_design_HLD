# SQL vs NoSQL

**Topic**: SQL vs NoSQL Databases

## Yeh kya hota hai? (What is it?)
Jaisa ki humne dekha ki database data store karne ke liye hota hai. Yeh do tarah se store kiya ja sakta hai: **SQL (Relational)** aur **NoSQL (Non-Relational)**. In dono mein difference samajhna System Design mein sabse bada decision hota hai.

### 1. SQL (Structured Query Language)
* Yeh Data ko **Tables** (Rows & Columns) ke form mein store karta hai.
* Iska **Schema** fixed hota hai. Matlab agar aapne table banayi `Users (Name, Age, Email)`, toh naye user mein sirf yahi 3 cheezein daali ja sakti hain. Agar Mobile Number daalna ho, toh pehle puri table ka structure modify (alter) karna padega.
* In table ke beech mein Relation (jaise Primary Key, Foreign Key) hota hai.
* **ACID properties** badi strongly follow ki jaati hain (Transactions humesha safe aur reliable rehti hain).
* **Examples**: MySQL, PostgreSQL, Oracle, SQLite.

### 2. NoSQL (Not Only SQL)
* Yeh Data ko Tables mein NAHI, balki **Documents (JSON)**, Key-Value pairs, ya Graphs ki format mein store karta hai.
* Iska **Schema** flexible (dynamic) hota hai. Ek product ke record mein aap 'Color' ko dal sakte hain, aur dusre product ke record mein aap bina usko declare kiye skip kar sakte ho.
* Yeh Horizontal Scaling ke liye behadh perfect hote hain.
* **Examples**: MongoDB (Document), Redis (Key-Value), Cassandra (Wide-Column), Neo4j (Graph).

## Example Scenario
**Kab Konsa Use Karein?**
* **SQL Example**: Agar aap ek Banking System (jaise State Bank of India) ya Financial App bana rahe ho jahan par transactions (paise bhejna/lena) 100% accurate hone chahiye (ACID compliance), wahan aap **SQL** use karoge. Yahan data ka format fix hai, 'Account balances' vagera perfectly relate hone chahiye.
* **NoSQL Example**: Agar aap Amazon ya Flipkart ka E-Commerce Catalog bana rahe ho, jahan har product (Shirts, Laptops, Books) ki alag-alag properties hoti hain. (Shirt ka size hota hai, par Laptop ka RAM hota hai). Ek fixed table handle nahi kar payegi. Yahan hum **NoSQL (MongoDB)** use karte hain taaki data flexibility bani rahe.

## Key Points
* **SQL**: Fixed Schema, ACID, Vertical Scaling (yadatar), Relational.
* **NoSQL**: Dynamic Schema, Base theorem, Horizontal Scaling, Non-Relational.
