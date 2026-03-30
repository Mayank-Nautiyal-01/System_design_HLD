# Denormalization

**Topic**: Database Denormalization 

## Yeh kya hota hai? (What is it?)
Normalization toh aap jante honge, SQL me tables ko chhote hisso mei baat dena taki double-double dupliate entry (redundancy) na ho! (Jaise User Table alag... User_Address Table alag). Perfect and Neat table-structure. Soch ek dam Clean (Codd's Rule in SQL). Par HLD aate tak ye baat samajh aati hai ke Database to DB-engine hi janta h, users k liye System ki speed (Performance/Read queries) boht main hoti h!

Normalization me "Read queries (SELECT)" ko 3 Tables Join marke sab result banana hotay (mushkil r mehnga kaam CPU level par).

Iska solution "Reverse Process" me hai jisko **Denormalization** kehte hain. Yeh ek jan-bhuj ke ki gayi technique hoti hai jisme Database Tables mein purposely, carefully **Duolicate-Data** dala jata hai, sirf aur sirf reads fast karne and "Joins ko kam/Kamzor" karne ke liye! Yaani System architecture mein extra disk-space ko hum 'Trade-off' karte hain (kurban kartay hah) FAST SPEED padne k lie!

## Example
**Table 1: User** `(ID, Name, Branch_ID)`
**Table 2: Branch_Info** `(Branch_ID, Branch_Name, Location)`

Ab ek profile page Load hota hai: "Hi Rahul (Mumbai)". Humara SQL is request ko laane ko dono k beech INNER JOIN lagata h. Lgataar har request pe ye calculations hoti hain. Normal h? YES. 
PAR, jab DB me scale pe ho. Kya 'Mumbai' bar bar fetch hga as common? JOIN server ki CPU khayenga!

Chalo **Denormalize** kre? Hum Table-1 ko thoda fat/ganda banadege (Add duplicate fields back again jisse 3NF k rules toote hon!):

Nayi **Table 1: User** `(ID, Name, Branch_ID, Branch_Name, Location)`

Fayda (Pros): Ab single Query hit hogi bas aur DP display hojaygi, without any JOINS! (Speed level max hojaygi).  And yes, "Branch_Name" ab do tables mei h- Ye Duplicate/Redundant hogya h lakin purposely. Jo chalega kyuki padhnha zyada jarrori tha is scale mein. 
(NOTE: Ab update boht dhyan se manage krna hoga application code mein backend ke throw). 

## Key Points
* NoSQL databases humesha default pe naturally "Denormalized-Format" (Documents object) approach hi follow karte hain! Jaise MongoDB mein. SQL me ise banaya/toda jata hai logic.
