# Caching

**Topic**: Caching 

## Yeh kya hota hai? (What is it?)
Imagine kijiye ki aap ek sawal puchte hain, internet use process karta hai, backend database se data nikalta hai, calculations karta hai aur response deta hai. (Time consuming - Latency). Par kya hoga agar **wahi sawal log 100 baar pochein**?
Kya bar bar (repetitive) DB se hit and mehnat karni chahiye? NAHI! 

Yahan **Cache (Temporary Fast Memory)** kaam aati hai. Jab koi pehli baar request laya tha tab humne uska final Output us tej chalne wale Memory Database (jaise Redis ya Memcached) mein temporary save/copy kar liya tha.  Agle user ke same query lane par system asal 'Main Database' tak gaya hi nahi, usne sidha Cache Memory check ki, waha anwser milgya aur Fata-fat (In milliseconds) de diya!

## Example
Aap Cricbuzz par "World Cup Final Live Score" dekh rahe hain.
Ye baat tay hai ki aapke ilawa **10 Lakh saavdhaan log** bhi us particular page k live score refresh mar raha honge every minute p.

Abt Cricbuzz ke original Database server par har second agar 10 Lakh SQL Query jayen (Select score where match_id="xx") to DB crash hoke behosh ho jayega (ya query delay banegi).
Un hone beech me **Cache** (In-Memory Data Store like Redis) lga diya. Har 10 second k bad DB ek updated score nikalta hai aur Cache me chipka deta hai. Ab wo "10 lakh/sec requests" aakar Cache ko milti hain (kyoki Redis sidha RAM se padhta hai - lightning fast). Aur database puri tarah free, cool aur safe hai peeche aram se letkar naye boundaries k waiting me! 

## Important Concepts
1. **Hit vs Miss**: Jab app Cache me dhundhne gayi aur output mil gya = "Cache Hit". Nhi mila (Toh main store jake fresh nikalega and khudme update karega) usko kehte hain = "Cache Miss".
2. **Eviction Policy (LRU/TTL)**: Cache memory RAM pe chalti hai aur mehngi (expensive) and choti hti hai. Full hone lagti hai! Toh purane/faltu results delete v to karne hai? Examples are "Time to Live"(TTL: 5 min bad udega) ya "Least Recently Used" (LRU: Jo chiz last 1 week se kisi ne query m nhi mangi uska output remove and make space).
