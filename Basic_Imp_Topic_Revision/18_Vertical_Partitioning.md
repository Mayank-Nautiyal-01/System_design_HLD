# Vertical Partitioning

**Topic**: Vertical Partitioning 

## Yeh kya hota hai? (What is it?)
Samajhiye humne pehle "Sharding (Horizontal Partitioning)" dekha jahan **Rows** kaat kar alag kiye gaye (A-H waale users alag, I-P waale alag).
Par **Vertical Partitioning** bilkul ulta hai: Ise Table ki **Columns** (Fields) ke hisaab se kaata jaata hai!

Jab ek table mein columns hi itne zyada (say 50-100 columns) ho jayen aur usme se humein pata ho ke roz roz read hone wala data kaun sa hai; aise mein us badi table ko choti table mein todd dete hain (divide kar dete hain). Voh alag alag Database servers mein bhi rackhi jaa sakti hain. 

## Example
Ek badi Ecommerce website par `Product` table hai. Ek single row (jaise Smartphone X1) mein kya kya columns hote hain?
(ID, Name, Price, Main_Image) -> Jo list page par dikhane chahiye sab users ko constantly.
Aur baki (Full_Description, Zoomable_Images, User_Manual, Weight_of_box, Guarantee_Terms) -> Ye rarely log tab use karte hain jab Detail page pe click karega. 

Aap ise **Vertical Partition** karenge (Columns split).
1. **DB Server A**: ID, Name, Price, Main_Image. Ye halke phulke hain aur search hone waale page yahan point kardete hain. Ye read heavily hote hain.
2. **DB Server B**: Baki lambi bhari Description & Manuals yahan rakh liye (Dono jagah 'ID' same rehga connection k liye). Request jab 'Know More' dabe tab is Database ko hit karti hai!

## Pros & Cons
**Fayde (Pros)**:
* Table structure bohot optimized aur compact ho jata hai, jisse important data easily Cache memory mein fit baithta hai aur website ki list super-fast load hoti hai. Storage space (RAM level) aur processing dono ki bachat.

**Nuksan (Cons)**:
* Agar aapko purana poora product ikattha show karna pada (ek PDF format output karne k liye) to wapis JOIN lagana padega jo slow and mehanga operation hota hai across multiple vertical splits ya servers mein se.
