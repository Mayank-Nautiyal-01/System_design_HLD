# API Gateway

**Topic**: API Gateway

## Yeh kya hota hai? (What is it?)
Agar aapke system mein saari 20+ Microservices hain (User, Catalog, Payment, Cart). Aur UI / Mobile Client aapke sabhi microservices ko directly unke alag URL par query bhej rahi hai:
`payments.myapp.com/pay`
`users.myapp.com/login`... toh ek waqt bad Network complexity app level pe itni gndi hojygi aur maintaince mushkil. Plus 20 alag service me Authentication checks 20 bar likhna (Duplicate Code)!

Bahar ke clients (Internet/Front-end) ko in internal backend Microservices se jhudne k liye bichari direct connection allow ni karte, balki **EK SINGLE ENTRY POINT / DOOR** banate hain backend ke face pe, Jise **API GATEWAY** kaha jata hai! Ek Front-Desk Receptionist ki tarah.

Sabki requests pehla gateway par aati hain, Gateway pehle dekhta hai - Route kahan jana chahiye? And andar bhej deta h. 

## Example
Manlo aap 5 Star Hotel me jate hain aur aapko direct 'Chef' se khana chahiye, 'Dhobi' se kpde and 'Cleaning staff' ko room bulana hai? 
App 3 alag-alag logo ko direct number milaoge and dhudhoge kahan kaam krrrhe hn voh hotel me = Wrong! (Microservices Direct Call).

Instead tum Hotel ki **Reception Desk (API GATEWAY)** ka number milate ho. Receptionist aapse poochegi - Aap authenticate client ho ya chori chhupe aye ho Room Card dikhao? (Authorization).
Phir wo tumhara instruction internal specific chef ya room-cleaning wale bande (Microservice) tak apne aap bhejegi. (Routing)! Tumhare client ka sir-dard khatam.

## Pros & Cons (Key Features)
**Gateway k 4 Main Tasks**:
1. **Routing**: Frontend se aane wali request ko `/pay` aya to Payment-Service ko bhejdena.
2. **Authentication**: JWT Tokens ya OAuth verify krna. Har app ko checking nahi krni hai ab. 
3. **Rate Limiting**: Hacker se DDoS rukna pehle darwaze p aage nhi.
4. **Logging & Analytics**: Kon user kis file k piche padi hai iski report!

* Example Products: Amazon API Gateway, Kong, Apigee (By Google Cloud).
