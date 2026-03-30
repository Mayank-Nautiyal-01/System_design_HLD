# CAP Theorem

**Topic**: CAP Theorem

## Yeh kya hota hai? (What is it?)
Jab hum Distributed Systems (yani jab data ek se zyada servers par rakha ho) ki baat karte hain, toh ek bahut famous rule apply hota hai jise **CAP Theorem** kehte hain. 

Ye theorem kehta hai ki in 3 cheezon mein se ek distributed system mein aap ek time par **sirf 2 hi cheezein 100% guarantee kar sakte ho**:
1. **C (Consistency)**: Sabhi users ko ek time par bilkul saari machines (nodes) par same Data dikhega.
2. **A (Availability)**: System hamesha on rahega aur har request ka ek non-error response dega (Data purana bhi ho sakta hai, par response aayega zaroor).
3. **P (Partition Tolerance)**: Agar network issue ki wajah se do servers ke beech connections toot jaye (Partition), tab bhi system as a whole kaam karta rahe.

## Example
Manlo aapka ek Database hai (DB-Delhi, DB-Mumbai). Inke beech mein ek cable judi hai data sync ke liye. Network disconnect ho gaya (Partition - **P** hamesha chahiye hi distributed mein).
Ab ek user ne ek photo like ki aur DB-Delhi me Data chala gya, DB-Mumbai nhi phucha.
- Dusra user DB-Mumbai p aake DP check karta h:

> Ab ya toh aap chahenge ki wo user DP tab hi dekh paye jab dono ka connection jhud jaye - To **Consistency (CP)** choose kari (Wait karo error dikhao, galat/purana nahi dikhana!). Jaise Banking system mein.

> Ya aap chahenge ki user ko Error nahi dekhna chaie, wo purana DP count(1 like) hi dekh le beshak! Vo  **Availability (AP)** choose karta hai! (Instagram, Facebook me chal jata h delay). 

## Key Points
* Interview mein mostly Distributed Databases ko classify karne ko bolte hain. MongoDB / HBase aate hain **CP** ki category mein. Cassandra/Amazon DynamoDB aata hai **AP** ki category mein.
* Network Partition (P) ko System design me mankar (assume) chalna hi padta hai (kyuki internet/cable lines kab fail hkuchni ptna). Isliye choice sirf C aur A ke beech hoti hai!
