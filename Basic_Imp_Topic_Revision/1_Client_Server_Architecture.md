# Client-Server Architecture

**Topic**: Client-Server Architecture

## Yeh kya hota hai? (What is it?)
Client-Server Architecture ek aesa model hai jahan do alag-alag parties hoti hain: 
1. **Client**: Jo request karta hai (jaise aapka web browser ya mobile app).
2. **Server**: Jo us request ko process karta hai aur response deta hai (jaise Google ya Facebook ka backend computer).

Aap isko ek restaurant ki tarah samajh sakte hain. Aap (Client) waiter ko order (Request) dete hain, aur kitchen (Server) aapka khana (Response) taiyar karke aapko deta hai.

## Example
Jab aap apne browser mein `www.google.com` type karte ho aur Enter dabate ho, toh kya hota hai?
- Aapka browser **Client** hai. Woh Google ke **Server** ko ek request bhejta hai ki "Mujhe apna homepage dikhao".
- Google ka Server us request ko dekhta hai, database se data uthata hai (agar zaroorat ho), aur wapas ek HTML page **Response** ke roop mein bhejta hai.
- Aapka browser us HTML response ko render (draw) karke aapko Google ka page dikha deta hai.

## Key Points:
* **Centralized Data**: Sara important data aur logic server par secure rehta hai.
* **Scalability**: Agar client zyada badh jayein, toh hum aur servers add kar sakte hain.
* **Separation of Concerns**: Client side (UI/UX) aur Server side (Business Logic) independent hote hain, unhe alag-alag develop aur update kiya ja sakta hai.
