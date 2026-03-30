# Horizontal Scaling

**Topic**: Horizontal Scaling (Scale Out)

## Yeh kya hota hai? (What is it?)
Jab humne Vertical Scaling dekhi, toh usme hum ek hi server ki takat badha rahe the (jo ki limit pe aake ruk sakti hai). Iska alternative aur far better approach hai **Horizontal Scaling**.

Iska matlab hai ki bajaye ek single machine ko bada banane ke, hum **aur naye servers (machines) add kar dete hain** apne pool/network mein. Isko "Scaling Out" bhi kehte hain. 

## Example
**Life Example**: Aapka ek chota sa restaurant hai wahan 1 waiter (Server) bahut achhe se kaam kar raha tha. Par phir bahut saare log aane large, aur wo 1 waiter sabhi table ka order nahi le paa raha (Slow/Crash).
* Vertical Scaling mein = aapne us same waiter ko roller-skates pehna diye aur red-bull pila di (Power badha di). But uski bhi limit hai.
* **Horizontal Scaling mein** = aapne usko help karne ke liye **4 naye waiters (servers) hire kar liye**. Ab sab log mil-baant kar kaam kar sakte hain. Jab traffic badhega, toh bas naye waiters (machines) add karte jao.

## Pros & Cons
**Fayde (Pros)**:
* **No Limit**: Aap theoretical roop mein 1000 ya 10,000 servers add kar sakte ho! Netflix, Facebook isi pe chalte hain.
* **Reliability (No Single Point of Failure)**: Agar ek server/machine kharab bhi ho gaya, toh dusre servers kaam sambhal lenge aur aapki app aaram se chalti rahegi.

**Nuksan (Cons)**:
* **Complexity**: System setup karna bahut mushkil ho jata hai. Data ko multiple machines ke beech mein sync rakhna aasan nahi hai.
* **Load Balancer Needed**: Agar 5 servers hain, toh user ki request aakhir jayegi kis waale ke paas? Ye manage karne ke liye ek alag software ya hardware lagana padta hai jise "**Load Balancer**" kehte hain (jo next topic mein aayega!).
