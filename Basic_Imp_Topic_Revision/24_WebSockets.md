# WebSockets

**Topic**: WebSockets

## Yeh kya hota hai? (What is it?)
Normal tareeke mein Client internet se Server ko 'HTTP request' marta hai, server reply deti hai, aur connection tut kar Khatam (Close) ho jata hai. Ab jabtak user dubara refresh nah dabaye, server khud baat nahi kar sakta (Unidirectional)!

Agar Humein Real-time baat-cheet (Live Action) karwani ho, waha **WebSockets** aata hai. WebSocket ek baar network par connection establish kare toh vo use **Open / Zinda (Persistent)** rakhta hai dono taraf se (Bi-Directional). Ab tak jabtak dono me se koi ek na kaathe, ya app band na ho — Server aapko direct live notification or message phek sakta h bina refresh maare! 

## Example
1. Aap normal 'Amazon pe order-history' dekh rahe hain = API Refresh based (HTTP call).
2. Aap apne doston ko **WhatsApp** ya Web-Chat par live msg kar rahe hai, screen ke ooper "Typing..." likhe horahe hein, aur Bina App refesh aapke dost ka message screen pe popup hojata hai = Yehi magic **WebSockets** kehte hain. 

Jaise Stock-Market Trading graph up and down (Live-ticking) hota hai. Web-socket mein 'Tunnel / Pipeline' khuli rakhi jaati hai taki real-time data millisecond me continuously push hota rahe.

## Pros & Cons
**Fayde (Pros)**:
* Real-time Data processing possible hoti hai (Multiplayer Games). 
* Bar Bar Request/Response Header ka extra size nahi bhejna padta to connection boht light speed / Efficient hota h HTTP polling se muqable. 

**Nuksan (Cons)**:
* Live Connecion State maintain krna Server ke CPU & RAM ko lagatar khata hai. Isliye isko Horizontal-Scale karna boht tough aur mehnga reheta hai!  (Sticky session LBs chhaiye).
