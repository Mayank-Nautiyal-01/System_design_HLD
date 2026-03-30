# APIs

**Topic**: APIs (Application Programming Interface)

## Yeh kya hota hai? (What is it?)
API ek aesa zariya hai jisse do alag-alag software applications aapas mein baat (communicate) kar sakte hain. System design mein API decide karta hai ki ek system dusre system se kaise data mangega aur dusra system usko us form mein data kaise return karega.

## Example
Sikhiye isko phir se waise hi **Restaurant** ke example se:
* Aap (User) table par baithe hain aur menu card dekh kar khana choose kar rahe hain. 
* Kitchen (Server) jahan khana banega.
* Ab problem ye hai ki aap direct kitchen mein jake order nahi bana sakte, aur kitchen khud aake aapse nahi puchta. Inke beech mein kon hai? **Waiter**. Waiter yahan **API** hai.

Waiter (API) aapka order leta hai, kitchen mein lekar jata hai, aur kitchen jab khana bana deta hai toh waiter (API) use wapas aakar aapko serve karta hai.

## Tech Example
Aap apne phone par **MakeMyTrip** app khol kar usme flight details (jaise Delhi to Mumbai) search karte hain. MakeMyTrip ke paas apni khud ki flights toh hain nahi.
Woh kya karega? Wo sidhe Indigo Airlines, AirIndia, ya Vistara ki alag-alag aapsi "**APIs**" ko request bhejega aur puchaega - "Bhai, is tareekh ko kitni flights hain? Aur ticket kitne ki hai?"
Phir un airlines ke server unki list bhejenge API ke zariye, aur MakeMyTrip app un sab list ko ek jagah present karke aapko dikhayega.

## Key Points
* API systems ko loosely coupled banata hai.
* Ek API ka kamal hoti hai ki wo internal details chupata hai, bas kya input chahiye aur kya output dega, itni baat batata hai.
