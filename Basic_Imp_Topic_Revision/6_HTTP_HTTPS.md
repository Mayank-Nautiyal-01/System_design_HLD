# HTTP aur HTTPS

**Topic**: HTTP / HTTPS (Hypertext Transfer Protocol / Secure)

## Yeh kya hota hai? (What is it?)
**HTTP** aur **HTTPS** do languages (protocols) hain jo aapka browser (Client) aur website (Server) aapas mein baat karne ke liye use karte hain. 

* **HTTP**: Ye ek basic rulebook hai Internet par data transfer karne ke liye. Problem ye hai ki isme jo data flow hota hai, wo *Plain Text* mein hota hai. Iska matlab koi bhi beech mein aake (Hacker) us data ko read kar sakta hai.
* **HTTPS**: Ye HTTP ka 'S'ecure version hai. Isme data bhejne se pehle usko lock (Encrypt) kar diya jata hai. Agar koi hacker beech mein data chura bhi le, toh usko bas random kachra (jumbled text) dikhega, kyunki key sirf client ya server ke paas hoti hai.

## Example
**HTTP ka example**:
Aapne ek post card likha apne dost ko aur Dak-Khane (Post office) mein daal diya. Beech mein postman ya koi bhi us post card ko aasaani se padh sakta hai. 

**HTTPS ka example**:
Aapne letter likha, usko ek lohe ke bakse (box) mein daala, aur us par ek lock laga diya jiski chabi sirf aapke dost ke paas hai. Ab postman usko carry zaroor karta hai, par ander kya likha hai wo nahi padh sakta.

## HTTPS kaise kaam karta hai?
Ye mainly **SSL/TLS Certificates** use karta hai data encrypt karne ke liye. Jab aap browser mein lock icon 🔒 dekhte ho url bar ke paas, uska matlab HTTPS activated hai.

## Key Points
* Aaj kal HTTPS mandatory (zaroori) hai. Chrome toh HTTP wali sites par direct warning de deta hai ("Not Secure").
* Secure communication ke liye hamesha HTTPS ka use karein, specially passwords ya credit card data bhejte waqt.
