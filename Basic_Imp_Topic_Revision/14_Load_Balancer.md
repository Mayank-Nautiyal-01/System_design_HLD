# Load Balancer

**Topic**: Load Balancer (LB)

## Yeh kya hota hai? (What is it?)
Jab aapne apna system "**Horizontally Scale**" kar liya yani ek server ke bajaye 5 servers laga diye. Ab users ki hazaro requests aayin hain. Ab un requests ko ye nahi pata ki kis DB ya Server ke paas jaana hai. 

Yahan **Load Balancer** ka kaam aata hai! Yeh ek Traffic Police ki tarah kaam karta hai. Ye directly internet ke face pe khada hota hai aur backend mein rakhe servers ke beech mein aane wali requests (traffic) ko barabar (balance) tarike se distribute/baat deta hai. Taaki koi ek server over-load na ho jaye.

## Example
Aap kisi Airport ya badi Bank ki billing line mein khade hain. Aage 5 billing counters (Servers) hain.

Line ke starting mein ek aadmi (Load Balancer) khada hai. Wo dekhta hai ki Counter-1 par bahut log hain par Counter-3 khaali hai, toh woh agle naye aane wale customer ko ishara deta hai ki "Bhaiya aap Counter-3 par chale jao!".
Aise hi, wo sabhi incoming customers (requests) ko intelligently distribute kar deta hai taaki sabko service fast mile.

## Load Balancer ke kaam (Key Features)

1. **Traffic Split**: Incoming requests ko `Round-Robin`, `Least Connections` jaise algorithms ka use karke bhejta hai.
2. **Health Check**: LB lagatar piche baithe servers ko message bhejta rehta hai ("Bhai theek ho?"). Agar server-4 ka response nahi aaya matlab wo down/crash ho gaya. Toh Load Balancer uss mrit(dead) server ke paas traffic bhejna band kar dega till wo fix na ho jaye. (Reliability badh gayi!).
3. **Security (Reverse Proxy)**: Hackers ko aapke real backend servers ki IP address nahi dikhaai degi, unhe sirf LB dikhega (Jo ki Reverse Proxy ki tarah se DDoS attacks bhi manage kar sakta hai).

## Key Points
* Cloud Providers inko pre-built dete hain (Jaise AWS mein ALB/ELB, Nginx OpenSource hai).
* Zyaada traffic wali applications ke liye Load Balancer first zaroorat (prerequisite) hota hai System Design mein.
