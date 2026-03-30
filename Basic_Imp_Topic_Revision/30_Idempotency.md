# Idempotency

**Topic**: Idempotency (in APIs & Distributed Systems)

## Yeh kya hota hai? (What is it?)
Mathematical / Database operation Idempotent 'Toh tab khlata hah' = **Agar aap ek hi Operation / Request ko bar_bar (1 ya 10 barr bhi) lagatar Retry perform karein, toh "Target ka RESULT/State hamesha WAHI hona chahiye jo 1st baar par aana tha"**. 

Bina Idempotency ke duplicate entry ban jaigi aur data corrupt (ya Paise Double cut jana bank account system m) ka risk hotha hah. Network mein Timeout pakka aati hai (Latency / drop). Jab Timeout aya to server kya reply dega app ko? Error! App dobara button/page refresh/retry dabaya to command "DUBARA" Server tak bhag ke gyi. Agar Server logic Idempotent na ho t double nuqsan. 

## Example
**Non-Idempotent (Khatarnak) Example**:
Tumne Uber pe car book karni h! "Pay $100 & Book" ka button click kara aur Internet chala gya bech mein Loading par! (Tume msg error mila "Timeout"). Par background me Payment deduction chal rha tha backend k!
Jab 1 Min bad Tumne dobara net aane pa button click mara -> Ab tumhare Debit/Credit card se **2 baar (100+100)** minus hogaye aur 1 driver k bdla 2 cars aagayi! "Galat / Flawed design System".

**Idempotent (Perfect HLD System) Example (Stripe Payments jesa)**:
HTTP API bhejte waqt Client Apni Request k sath ek Unique "*Idempotency-Key (UUID)*" generate kr ke bheja!
Pehli Baar => `POST /book {"key":"A1X"... }` Database me entry save "Key:A1X -> Status Paid $100!".  
Agar Timeout laggya aur tumne Firse Dabak kr retry kya = To firs same request gayi > `POST /book {"key":"A1X"... }`.
Par isbaar API Payment provider bolega... "Wait! Mere data me 'A1X' naam ki Key already PICHLe min aa chuki ha na is user se? Mujhe usne pehlay success record hai - MEi PAISE Nahi cut krunga - is bnde ko Firse 'Success Status = Paid' dikha do!"
Result: User ke bank se sirf $1 बार Kate, app safe ha! Ye 'IDEMPOTENT' operation tha!

## Key Points
* Hamesha Retries (Jaise Microservice calls) mein **Idempotency Keys** k implementation dhyan m rakhni chhiye. HLD mein API ko Resilient (mazboot banati h). 
* Data state same rehna uske baad. GET and PUT aur DELETE, HTTP Methods idompotent k design hote hain (Pehle C-delete hui tbhi 404 baram bar). Par `POST` default se Idempotant nhi hua unse double list bani hmesah. 
