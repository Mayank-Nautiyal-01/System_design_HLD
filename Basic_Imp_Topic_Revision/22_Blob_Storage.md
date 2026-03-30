# Blob Storage

**Topic**: Blob Storage (Object Storage)

## Yeh kya hota hai? (What is it?)
Humne abhi tak Databases mein Text/Numbers store karna dekha. Par aap WhatsApp par jo Image/Audio bhejte ho, YouTube par jo Videos upload hoti hain, ya PDFs, wo kahan store hote hain?

Unke liye conventional SQL/NoSQL databases bilkul theek nahi hain (database bloated ho jayega). Un sab media files aur unstructured bade data ko store karne ki special jagah ko **Blob (Binary Large Object) Storage** kehte hain. Isay Object Storage bhi kaha jata hai.

Yahan data "Objects" ki form mein Save hota hai. Har ek file ek object ban jati hai aur usay ek unique ID ya 'URL' milti hai taaki direct internet se access kiya ja sake.

## Example
Aap apne college ka form bhar rahe hain aur "Resume.pdf" upload kiya.
Peeche form server uss Resume ko database (MySQL) mein column entry nahi karega, wo use sidha cloud mein ek container (Amazon S3 ya Google Cloud Storage - dono Blob storage k examples) mein push kar dega. Wapas ek chhota sa link milega (`https://s3.bucket.com/resume_xyz.pdf`). 
Phir database mein wo API sirf is **LINK (URL)** ko text ki form mein save kar legi apne column me! Database ko files ka wajan/bhaar nahi uthana padta.

## Key Points
* **Scalable**: Cloud pr Blob storage (like AWS S3) technically unlimited storage capacity deta h!  Duniya ka sabse sasta cloud solution bnta hai (Per GB storage cheap hoti h).
* **Flat Structure**: Files folders/tree format mein nahi hote, ek badi sapat (flat) array hoti hai aur metadata ke sath sab sath sath rakhe jate hain.
