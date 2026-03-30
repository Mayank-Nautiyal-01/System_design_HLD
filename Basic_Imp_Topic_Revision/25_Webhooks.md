# Webhooks

**Topic**: Webhooks (Reverse API / User-defined Callbacks)

## Yeh kya hota hai? (What is it?)
APIs me humne dekha ki ek system dusre system ke paas jata hai puchte hue: "Are bhai update aya kya?". Ise bolte hai Pulling data (har 5 minute app call kar rahi hai server ko!).

**Webhooks** ek Push mode ka approach hai! Webhooks ka seedha sa matlab hai "Don't call me, I will call you". 
Tumne ek jagah apna Mobile number (URL ya Endpoint jahan message hit ho) dekar subscribe kr diya. Jab EVENT asal mein hit hoga wahan, wo service tumhein message/notification khud bhejegi via HTTP POST. Vo hai apna Webhook! Yaha apko lagataar API calls karke CPU waste nhi krna! Event-Driven approach. 

## Example
Ek Badi Online E-Commerce site setup ki apney aur usme Razorpay / Stripe ka Payment Gateway lagaya. 

Ab koi user "Pay-Now" karta hai aur OTP dalke Net-banking se pay karta hai UPI app me.  (Asynchronous Task). 
Aapki Website bar-bar us Bank ko poochti rahe 2 second me (Polling)? Nahi!
Uske bdle Payment provider ek **Webhook** mange ga (ek URL code like `https://yoursite.com/api/payment_success_hook`). 

User ne mobile par complete ki transaction, Razorpay apne end se trigger chalakar sidhe tumhare us Webhook URL ko ek signal POST bhejega `{"status" : "SUCCESS", "amount": 500, "OrderId" : "12X"}`!
Ye direct PUSH notification mila aur appke service me Invoice bill generate ho gaya automatically and process successful!

## Key Points
* Webhook ek user-defined HTTP callback he jo kisi external source me Action hone par initiate hota hia.
* Isme Server resource bachti hai HTTP Polling k muqable! (GitHub Webhooks or CI/CD pipelines is pure hook base).
