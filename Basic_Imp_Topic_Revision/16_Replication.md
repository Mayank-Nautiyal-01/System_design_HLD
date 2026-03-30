# Replication

**Topic**: Database Replication

## Yeh kya hota hai? (What is it?)
Agar aapka saara data ek hi Database server par rakha hai, toh kya hoga agar uski hard disk jal jaye ya server crash ho jaye? Aapka saara data hamesha ke liye chala jayega.

Isko bachane ke liye (Reliability badhane ke liye), hum **Replication** ka use karte hain. Iska matlab hota hai ek hi data ki **multiple copies (photo-copies)** alag-alag Database servers par rakhna. Ek server **Master (Primary)** ban jata hai, aur baaki **Slave (Secondary/Replica)** ban jate hain.

## Master-Slave Architecture (Example)
Aapne WhatsApp par apni Profile picture change ki (Write Operation). Yeh data aake **Master Database** ko modify karta hai. Phir turant (almost instantly) Master us naye profile picture ko apne **Slave/Replica Databases** me bhi copy (sync/replicate) kar deta hai. 

* **Master ka kaam**: Naya data Add/Update/Delete karna (Write operations).
* **Slaves ka kaam**: Data read karna. Jab users aapki DP dekhte hain, tab request sirf aur sirf Slaves pe jayegi (Read operations).

**Kyun?** Kyunki kisi bhi system mein 95% request Data padhne ki hoti hain aur sirf 5% likhne (update) ki.

## Pros & Cons
**Fayde (Pros)**:
* **High Availability**: Agar ek Master server crash hota hai, toh System design apne aap ek Replica/Slave ko naya Master promote kar denga, yani no down-time! Data Safe hai!
* **Read Scalability**: Hum 10 Replica server laga sakte hain read operations ke liye jisse Load Balancer traffic split karke database ki speed bahut fast kar deta hai.

**Nuksan (Cons)**:
* **Storage cost**: Ek hi data 5 jagah kyu rakha? Disk space mehengi zaroor padhti hai.
* **Replication Lag**: Master ne toh Data update kar liya lekin abhi tak Slaves ko copy send nah hui, toh us beech wale micro-seconds me agar kisi user ne purana data read kar liya (Stale Data/Inconsistency). Ye ek choti si problem hoti hai jo System Design me face karni padti ahai.
