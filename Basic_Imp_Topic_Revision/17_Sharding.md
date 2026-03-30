# Sharding

**Topic**: Database Sharding (Horizontal Partitioning)

## Yeh kya hota hai? (What is it?)
Pichle topic (Replication) me humne ek hi table (100 GB) ki exact 3 copies banali thi alag-alag machines par, but overall ek server ke paas pura data tha!

Par jab aapka Data 10,000 GB (10 TeraBytes) ya usey v bada ho jaye? Ek single machine (Hardware) itna saara data physically apne ander store aur manage NAHI kar sakta chahe usko kitna bhi Vertically Scale kyu na kiya jaye. Is problem ka solution hai **Sharding**. 

Sharding ka sidha matlab hai **Data ke tukde (Pieces) karke alag-alag machines pe daal dena**. Ek table ko Rows(Horizontal) ke basis pe kaatna.

## Example
1. Aapke User table mein total **10 Crore Users (100 Millions)** ka record tha. 
2. Ab search jaldi ho, toh aapne kya kiya?
    * "A se leke H" naam wale 3 crore users ka data **DB-Server 1 (Shard 1)** pe dal diya.
    * "I se leke P" naam walo ka data **DB-Server 2 (Shard 2)** pe dal diya.
    * "Q se leke Z" walo ka data **DB-Server 3 (Shard 3)** pe dal diya.

Aapne data ke Shards bana diye. Ab koi "Karan" ki query karta hai, to system ko sirf "Shard-2" (Machine-2) se baat karni padegi, usko 10 crore records mein nhi ghumna padega as total usme bas 3cr record honge!. Speed up, memory solved! Usse "**Partition Key**" ya "**Shard Key**" bolte hain jiske basis par system decide karta hai traffic kahan point karega (Yahan Initial Letter ya UserID).

## Pros & Cons
**Fayde (Pros)**:
* Huge scale databases sirf sharding pe hi ticke hote hain (Infinite scalability database pe).

**Nuksan (Cons)**:
* **Complexity**: Konse shard me data jayega / aayega uska logic API pe clear and powerful likhna padhta harna wrong server hit hoga (Jo ki app level ya API load-balancer handle karega).
* **Joins**: Agar do users alag alag Server/Shard mein bethe hain (Jaise 'Amit' Shard 1 aur 'Rajat' Shard 3) par unka connection banana hai ya unpe JOINS operations lagane hain toh vo cross-node connection expensive and very slow hota hai (Dono tables milake query marna mushkil).
