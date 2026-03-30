# Vertical Scaling

**Topic**: Vertical Scaling (Scale Up)

## Yeh kya hota hai? (What is it?)
Maan lijiye aapka ek server hai jo website host kar raha hai. Ab website par bahut saare users aane lag gaye aur aapka server crash ya slow hone lag gaya.

Isko theek karne ka ek tareeka hai **Vertical Scaling**. Iska matlab hai ki aap apne **USI SAME SINGLE SERVER** ki taqat (power) badha do. Yani usme aur zyada RAM lagwa do, aur behtar CPU (processor) laga do, ya Storage (SSD) badha do. Isko "Scaling Up" bhi kehte hain.

## Example
Aap apne purane 4GB RAM wale Laptop par ek naya heavy game khel rahe hain (jaise GTA 5). Aapka laptop lag karne laga, uski memory full ho gayi.
Aapne kya kiya? Aap dukan wale ke paas gaye aur apne laptop mein nayi 16GB ki RAM aur latest Graphic Card dalwa liya. Ab aapka game smooth chalne laga.

Yahan aapne naya laptop nahi kharida, balki purane ki hi power upgrade kar di. Yahi machine ya server ke perspective se **Vertical Scaling** hai.

## Pros & Cons
**Fayde (Pros)**:
* Yeh sabse asaan (Simplest) tarika hai. Aapko apna code ya system architecture badalne ki zaroorat nahi hoti. Database bhi wahi rehta hai, bas hardware bada ho jata hai.
* Data inconsistency ka koi chance nahi kyunki data ek hi server par hai.

**Nuksan (Cons)**:
* **Hardware Limit**: Ek limit hoti hai ki ek machine mein kitni RAM ya kitne CPUs dal sakte hain. Ek waqt aayega jab us machine ki maximum limit poori ho jayegi (jaise max 128GB RAM uske baad kya?).
* **Single Point of Failure**: Agar wo ek powerful server crash ho gaya (hardware issue ya power cut), toh aapki poori website down ho jayegi.
* **Downtime**: RAM aur CPU badhane ke liye aam-tor par server ko band karna (Restart) padta hai, jisse system thodi der ke liye offline ho jata hai.
