# Indexing

**Topic**: Database Indexing

## Yeh kya hota hai? (What is it?)
Jab humare Database table mein lakho/caroro (Millions) rows aa jati hain, toh search karne ki process (Query) achanak se bahut slow ho jati hai. Kyunki database ko ek single record dhundhne ke liye puri table ki line-by-line scanning (Full Table Scan) karni padti hai.

Isko theek karne ke liye hum table ke column par **Index** banate hain (Indexing karte hain). Yeh bilkul waisa hi hai jaise kisi moti si kitab (book) ke piche ek "Index section" diya hota hai.

## Example
Aap ek 1000 page ki English Dictionary lekar baithe hain aur aapko "Zebra" shabd ka meaning dhundhna hai. Aap har ek page nahi padhte na (Full table scan)? 

Aap dictionary ki "**INDEX**" (Ya uske alafabet tab) ka use karte hain aur sidhe 'Z' wale section (Page 990) par chale jaate hain. Fatafat aapko result mil jata hai!

Bilkul wahi concept Computer Database mein hota hai. Agar aapne `Employee` table ke `emp_id` par Index kiya hua hai, toh database us index (jo normally B-Tree ya Hash me store hota hai) ko dekhkar turant bata deta hai ke `emp_id=89233` Hard-Disk ke konse block/page par rakha hai!

## Pros & Cons
**Fayde (Pros)**:
* Data search (SELECT, READ queries) karne ki speed 100x ya usse bhi zyada badh jati hai. Performance boost!

**Nuksan (Cons)**:
* **Storage Space**: Index ko bhi memory me space lagti hai.
* **Write Query Slows Down**: Jab naya data add hoga (**INSERT**) ya purana update hoga (**DELETE/UPDATE**), toh Table ke sath-sath uske Index list ko bhi database ko update karna padega. Is se writing operations thode slow (heavy) ho jate hain. 

## Key Points
* System Design (HLD) interview mein aam sawal aata hai: "Agar Database read-heavy hai par slow padh gaya toh kya karoge?". Pehla simplest step: "Dhekunga ki proper Indexing hai ki nahi".
* Faltu har column par Index nahi banate, dhyan se soch kar banaya jata hai jahan WHERE clause zyada lagne ho.
