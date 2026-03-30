# Database

**Topic**: Database

## Yeh kya hota hai? (What is it?)
Agar simple bhasha mein kahein, toh **Database (DB)** ek aesi jagah hai jahan par humara system electronic form mein data (information) ko store, manage, aur organize karta hai, taaki baad mein usko asani se search, add, ya update kiya ja sake.

Har app ko data save karne ki zaroorat hoti hai. Jab user app load karta hai tab data wapis read karke screen pe dikhaya jata hai.

## Example
Aap apne ghar mein purani files aur documents rakhne ke liye ek "Filing Cabinet (Almirah)" ka use karte hain. Usme alag-alag darazein hoti hain: "Bank documents", "School certificates".
Waisi hi ek almirah aapka **Database** hai. 

Jab aap kisi e-commerce website par jate hain, toh "kis user ne kab kon-sa order place kiya tha", uski history safe rakhne ke liye software usko Database mein insert kar deta hai. Agar app apna password bhool gaye ho, toh system Database mein dekhta hai ki aapka email mojood hai bhi ya nahi.

## Different Types (Brief)
Do main tarah ke Databases use hote hain:
1. **Relational Database (SQL)**: Yahan Data exactly structured tables (Rows & Columns) mein save hota hai jaise MS Excel. (E.g. MySQL, PostgreSQL, Oracle).
2. **Non-Relational (NoSQL)**: Ye zyadatar unstructured ya flexible format mein data save karte hain, jaise Documents(JSON), Key-Value pairs waghera. (E.g. MongoDB, Redis, Cassandra).

## Key Points
* HLD (High-level design) mein database system ka dil hota hai (Single source of truth).
* Bina database ke application ka state/data server restart hote hi khatam ho jayega. (RAM volatile hai, Storage/Database non-volatile data-persistence ke liye hota hai).
