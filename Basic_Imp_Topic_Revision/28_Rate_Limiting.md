# Rate Limiting

**Topic**: Rate Limiting / API Throttling

## Yeh kya hota hai? (What is it?)
Aapne server set kiya aur aapki API `api.com/users` bahut fast answer de rahi hai. Lekin kya hoga agar koi Hacker wahan ek script start kar de jo ek hi second mein **1,00,000 requests (DDoS attack)** us API par bhej de?  Aapka server Memory Full hoke mar jayega (Crash) aur baaki sab ache Users ko error ayega! 

Aesi condition aur heavy load se apne API backend ko safe and secure rakhne k liye, "Darwaje par ek check" set kiya jata hai jise **Rate Limiting** bola jata hai. Yani hum set kardete hai k "Ek client/IP se ek Minute mein maximum 100 requests hi handle karunga", is rule break hue toh Server bina data process kiye seedhe HTTP Error `429 Too Many Requests` thop (block) dega! isee "Throttling" bhi bolte he.

## Example
Aapne ek free Weather API key li huyi hai RapidAPI platform se apna college project banane k liye! Waha wo dashboard p rule dete hain -> **Per Day sirf 50 requests allowed!**.
Aap application call me ek variable/key lagate hen. Jab apke app ne 50 bar function chalaya, unki Weather Server p API p pucha! Aur ap 51th time query maroge, unki Rate-limiting algorithm apko ek Error Return kr degi: "Plan/Quota Exceeded. Please upgrade to Pro for unlimited requests".

## Algorithms for Logic
Isey implement karne ke kuch popular Algorithms Hote hain In-Memory/Redis DBs me :
1. **Token Bucket** (Jaise Melle mein ride jhulne ki Tokens fixed mili hongi, khali hue to ride Nhi).
2. **Leaky Bucket** (Baalti me ek hole, incoming flow fast hai th bhi bahar fix-rate me dheere pani niklna).
3. **Fixed Window / Sliding Window Counter** (Ex: Per Minute IP ki count count krega aur block krega 60s k lie).

## Key Points
* Bahut Zaroori technique h DDos attack Bachane k. Load Balancer / API Gateways ke face par hi Limit configure ki jaati hah! (Internal Service me nhi!)
