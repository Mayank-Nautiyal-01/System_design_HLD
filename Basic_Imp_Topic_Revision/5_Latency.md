# Latency

**Topic**: Latency

## Yeh kya hota hai? (What is it?)
Simple shabdon mein, **Latency** ka matlab hai **Time Delay (Deri)**. 

System design aur Networking mein, Latency wo time hai jo ek data packet ko ek point (jaise aapka device) se dusre point (jaise cloud server) tak pahunchne mein lagta hai, aur wahan se response aane mein lagta hai. Isko usually milliseconds (ms) mein naapa jata hai.

Hum hamesha chahte hain ki humare system ki latency **Low** ho (kam deri ho), taaki sab kuch super-fast feel ho. Agar latency **High** ho jati hai, toh system aesa lagta hai jaise "lag" kar raha hai / atak raha hai.

## Example
* **Real-life Example**: Aapne kisi se ek sawal pucha, usne sunkar process kiya, aur phir aapko jawab diya. Aapke sawal puchne aur uske jawab dene ke beech mein jo 2-3 second ka gap aaya, wahi Latency hai.
* **Tech Example (Gaming)**: Aap PUBG ya koi online multiplayer game khel rahe hain. Aapne 'Fire' ka button dabaya, aur goli chalne mein screen par 1 second ka delay aaya, iska matlab hai ki server aur aapke computer ke beech latency high ("ping high") hai. Agar aapka server India mein hai toh latency bahut kam hogi (maybe 20ms), lekin agar server US mein hai, toh data travel karne mein time lagega aur latency badh jayegi (maybe 250ms).

## Latency vs Bandwidth vs Throughput
* **Latency** is like a distance - ek gaadi ko point A se B tak jane mein kitna 'Time' lag raha hai.
* **Bandwidth** is like width of road - kitni chodi sadak hai? Kitni zyada cars ek time par ja sakti hain.
* **Throughput** is actual result - 1 second mein asal mein kitni cars pass hui.

## Key Points
* Latency ko hamesha kam karna (minimize) koshish rehti hai.
* Isko kam karne ke liye hum CDN (taaki data user ke paas ho), Caching, aur efficient data structures use karte hain.
