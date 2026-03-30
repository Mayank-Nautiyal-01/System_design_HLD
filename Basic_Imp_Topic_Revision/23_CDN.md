# Content Delivery Network (CDN)

**Topic**: Content Delivery Network (CDN)

## Yeh kya hota hai? (What is it?)
Socho aapne apni ek website ka ek single server **Delhi, India** mein rakha hai. Koi India se visit karega to site 0.1 second me load ho jayegi. Par agar koi New-York (USA) ka user aapki photo open karna chahega, toh photo ka data poori duniya cross karke sea-cables se aayega, tab kahi Jake photo khulegi. Isme lagne wle **Latency** time ko CDN bachata hai!

**CDN** ek worldwide network hai **Proxy Servers/Cache servers** ka, jo duniya ke har kone (USA, Europe, Japan, India) mein physically spread out hain. Wo aapki website ka **static data** (jaise Images, Video Files, CSS, Javascript) apne paas copy (cache) karke rakh lete hain. 

Ab koi user kahi se content mangta hai, to woh Main-Server(Delhi) par load dalnay k baajaye apne sabse paas waale CDN Server(Jaise NYC Edge Server) se file fetch krleta hai turantt! Pyaara, speed-up network delivery.

## Example
Aap Netflix par "Stranger Things" dekh rahe ho?
Puri duniya ki movies ko Netflix ke original (say, California US) wale data-centers se india nahi bheja jaata daily basis pe, internet traffic se congestion marjayega!

Netflix Amazon/Cloudflare / CDN edge servers rent pe leta hai aur **Aapke shehar (Eg. Mumbai)** k pass wali building mein wo popular series ek cache-copy rkha deti hai CDN!  Aap jaise hi "Play" dabaynge, picture Delhi/Mumbai valay pass k server se stream hogi aur direct speed mileygi Bina Buffer kiye! 

## Pros & Cons
**Fayde (Pros)**:
* User Experience (Low Latency): Data physically user ke karib hota hai to ping less aata hai.
* Real server ki bandwith badti haih kyonki Heavy Files CDN server k dwara handle ki gayi hain!

## Key Points
* Static Content (Logo, Images, JS/CSS files) ke liye CDN lazmi/mandatory hai modern HLD system design me.
* CDN ke top providers: Cloudflare, AWS CloudFront, Akamai.
