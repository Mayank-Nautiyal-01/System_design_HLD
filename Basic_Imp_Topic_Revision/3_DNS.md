# Domain Name System (DNS)

**Topic**: Domain Name System (DNS)

## Yeh kya hota hai? (What is it?)
Internet par sabhi websites ka apna ek IP Address hota hai (jaise `142.250.190.46`). Lekin hum insaano ke liye itne numbers yaad rakhna mushkil hai. Hum names aasaani se yaad rakh sakte hain (jaise `google.com`).

**DNS** internet ki ek **Phonebook (Contact List)** ki tarah kaam karta hai. Ye insano ke smajhe jaane wale names (Domain Names) ko computers ke smajhe jane wale numbers (IP Addresses) mein convert karta hai.

## Example
Aap apne phone mein apne dost 'Rahul' ka number uske naam se save karte ho, na ki uske 10-digit mobile number se. Jab aap 'Rahul' ko call milate ho, toh phone apne aap us name ko number mein map karke call connect karta hai.

Bilkul waise hi, jab aap browser mein `www.youtube.com` type karte ho:
1. Aapka computer DNS Server ke paas jata hai aur puchta hai, "Bhai, `youtube.com` ka IP address kya hai?"
2. DNS Server apni list check karta hai aur wapas jawab deta hai: "Iska IP `142.250.67.206` hai."
3. Phir aapka browser direct us IP address wale server ko request bhej kar website load kar leta hai.

## Key Points:
* DNS humans aur computers ke beech ek translator hai. (Domain Name  ➡️  IP Address).
* DNS servers ek hierarchy follow karte hain (Root servers, TLD servers, Authoritative servers) taaki resolution fast ho sake.
* Har bar website visit karne par DNS lookup ka time bachane ke liye aapka browser aur OS IP address ko **Cache** kar leta hai.
