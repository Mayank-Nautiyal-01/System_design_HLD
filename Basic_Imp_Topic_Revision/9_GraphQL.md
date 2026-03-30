# GraphQL

**Topic**: GraphQL

## Yeh kya hota hai? (What is it?)
Jab REST APIs mein kuch problems aani shuru hui (jaise Over-fetching ya Under-fetching), toh Facebook ne 2015 mein ek naya tharik develop kiya jiska naam **GraphQL** hai.

GraphQL aapko power deta hai API se sirf utna hi data mangne ka, jitne ki exactly zaroorat hai. Na usse ek character zyada, na usse ek character kam. Aur ye sab sirf ek single endpoint (jaise `POST /graphql`) pe hota hai.

## The Problem With REST
Aapne Amazon par 'Laptops' search kiye aur REST API ne response diya - list of Laptops along with unki picture, price, reviews, seller details, saari history etc. Lekin front-page par aapko khali Laptop ka **Naam** aur **Price** dikhana tha. Ye **Over-fetching** hui (Zaroorat nhi thi utne data ki).

## Example
Ab dekhte hain same cheez GraphQL kaise solve karega. Client (apki app) bas ek specific Query server ko bhejegi:
```graphql
query {
  laptops {
    name
    price
  }
}
```
Laptops data model mein halanki 10 aur cheezein hain, but server aapko response mein sirf Name aur Price dega!

**Response:**
```json
{
  "laptops": [
    { "name": "Macbook", "price": "1,00,000" },
    { "name": "Dell", "price": "50,000" }
  ]
}
```

## Key Difference: REST vs GraphQL
* **REST**: Server decide karta hai ki konsa data wapis bhejna hai. Mulitple endpoints hote hain (`/users`, `/posts`).
* **GraphQL**: Client decide karta hai ki usko exactly kaunya kaunsa information chahiye. Ek hi URL/endpoint hota hai (`/graphql`).

## Key Points
* Mobile devices par internet slow ho sakta hai aur extra data down-load avoid karne ke liye GraphQL use hota hai.
* GrapQL mein graph structure hota hai toh complex relational database ko fetch karna assan rehta hai in single query.
