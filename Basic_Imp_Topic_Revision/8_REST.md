# REST

**Topic**: REST (Representational State Transfer)

## Yeh kya hota hai? (What is it?)
Aapne pichle topic mein API samjha tha. **REST (ya RESTful API)** API banane ka sabse popular tharik aur set of rules hai. Yah web services ko banana aasaan banata hai jahan hum internet (HTTP methods) ka asani se use karte hain data ko manipulate ya fetch karne ke liye.

REST mein sab kuch "**Resources**" (jaise User, Product, Post) ke aass-pass ghumta hai. Ek REST API humesha 4 main actions support karti hai, jisko hum **CRUD** operations kehte hain.

## CRUD kya hai?
Agar aapko database mein koi kaam karna hai, toh 4 actions lenge:
* **C**reate: Naya record banana (**POST** method)
* **R**ead: Records ko dekhna (**GET** method)
* **U**pdate: Purane record ko change karna (**PUT / PATCH** method)
* **D**elete: Record ko mita dena (**DELETE** method)

## Example
Maan lo aap Instagram jaisi ek app bana rahe ho. Agar aap REST rules follow karenge toh aapke API endpoints kuch is tarah dikhenge:

1. `GET /users` -> Saare users ki list lao.
2. `GET /users/123` -> Uss user ka data lao jiska ID 123 hai.
3. `POST /users` -> Ek naya user create karo account banane ke liye.
4. `PUT /users/123` -> ID 123 wale user ki puri profile udpate karo.
5. `DELETE /users/123` -> ID 123 wale user ka account delete karo.

## Key Points
* REST APIs hamesha *Stateless* hoti hain. Yani har request ke sath sara zaroori data bhejna padta hai, kyunki server pichhli request ka data yaad nahi rakhta.
* Ye bahut lightweight hoti hai aur response normally JSON (JavaScript Object Notation) format mein bhejti hai.
