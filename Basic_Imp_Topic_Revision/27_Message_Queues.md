# Message Queues

**Topic**: Message Queues / Pub-Sub (Asynchronous Processing)

## Yeh kya hota hai? (What is it?)
Maan lo aapne app me koi bohot **Heavy task** submit kiya, jisko pura karne mein system ko 5 min lagenge. Par user ki request ko hum 5 min tab load / intezar karwainge to app timeout/crash de dega!

System Design mein aese heavy operations/background processing ke liye humare pass **Message Queue (RabbitMQ, Apache Kafka ya SQS)** jaisi technique aati hai. Yeh ek "Asynchronous Message buffer" hai. Jisme hum requests (Messages) ko 'Publish' (daalna/Enqueue) kar dete hain ek line me aur wapas aakar bolte hai apne user ko "Bhai kaam queue main lga gya h, app ja ke chill maro jab hogya to msg ayega". Aur server ke background mein baaki Idle (khali) machines unko slowly, apne hisab sy queue se nikal k 'Consume' karti rehti hn.

## Example
1. Aap Instagram pe Account banate ho "Sign Up", ab appne submit dbaya! Iske peeche Microservices ko Account banan h, Email bhejna verification Link ka(slow task), SMS ka MSG (15 sec) lagna h. 
2. Agar Sab Synchoronous hota to UI loading loader ghumyga 20 sec tak (Boring).

Yaha pe Backend Server bas Data SQL database me likhi aur baaki ke slow task ko "**Message Queue**" (Kafka) me publish (daal kar) wapis "Response : OK Done!" de diya 0.1 second mein! (Fast speed).
Abhi background 'Queue' se "Email Processing Microservice" ne vo message consume kiya aur chup-chap 1 minute bad link ki mail bhej diya (Peeche k peeche, decoupled, reliably and Asynchronous).

## Key Points
* Message queues boht useful hote hah Microservices Architectures mein jisme **Decoupling** chhiye (Matlab send karna wala bhale hi msg chhod k chla Jaye, rcvd karna wala usey apni speed aur memory se nikalenga jab voh azad Hoga, Server over-load kabhi nahi Hoga!)
* **Kafka** ek bahut badhiya open source message broker (Stream handler) hai High Scalability ke liye popular.
