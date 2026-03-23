# Email Protocols

Email is one of the oldest and most widely used communication systems on the internet. In System Design, understanding how email systems work is crucial because almost every modern application relies on email for user registration, password resets, and notifications.

Unlike regular HTTP requests where a single protocol handles communication, email relies on a combination of different protocols to *send* and *receive* messages. 

---

## 1. SMTP (Simple Mail Transfer Protocol)
SMTP is the **Push protocol**. It is exclusively used for **sending** emails. 

When you want to send an email, your email client uses SMTP to push the message to your email server. Your server then uses SMTP again to push the message to the recipient's email server. 

### Key Characteristics:
- **Port:** Uses Port 25 (unencrypted) or Port 465 / 587 (secure/TLS).
- **Direction:** Always pushes data to a remote server. 
- **Limitation:** It cannot function as a highly interactive mechanism to retrieve or manage an inbox. 

*Example:* 
You compose an email on your Gmail app and hit "Send." The Gmail app uses SMTP to send the draft to Google's outgoing mail servers. Google's server then uses SMTP to push the email across the internet to the recipient's Yahoo server.

---

## 2. IMAP (Internet Message Access Protocol)
IMAP is the **Sync protocol**. It is used for **retrieving and managing** emails.

IMAP was designed for the modern era where users own multiple devices (a laptop, a tablet, and a smartphone). Instead of downloading the data and deleting it, IMAP syncs the state of the email directly with the server. 

### Key Characteristics:
- **Port:** Uses Port 143 (unencrypted) or Port 993 (secure/TLS).
- **Direction:** Pulls and synchronizes data.
- **Benefits:** Emails remain stored on the server until explicitly deleted. Read/unread status and folder organizations are synced everywhere.

*Example:* 
You read an email on your phone while commuting. Because your phone is using IMAP, it syncs the "Read" status back to the Google server. When you open your laptop later, that same email is already marked as "Read."

---

## 3. POP3 (Post Office Protocol version 3)
POP3 is the **Download and Delete protocol**. It is an older standard used for **retrieving** emails.

When a client uses POP3 to fetch emails, it downloads the emails locally to the device and immediately deletes the originals from the server (by default). 

### Key Characteristics:
- **Port:** Uses Port 110 (unencrypted) or Port 995 (secure/TLS).
- **Direction:** One-way pull and delete.
- **Drawbacks:** Very poor for multi-device setups because once an email is downloaded to your phone, it no longer exists on the server for your laptop to download. 

*Example:* 
An enterprise system uses POP3 for an automated script that securely fetches sensitive incoming daily reports, processes them locally, and permanently erases them from the mail server to free up space.

---

## Summary Flow: How an Email is Delivered
To understand how these protocols work together, follow the journey of a single email:

1. **Client A** (Sender) writes an email to **Client B** (Receiver).
2. **Client A** pushes the email to **Server A** using `SMTP`.
3. **Server A** pushes the email across the internet to **Server B** using `SMTP`.
4. The email arrives at **Server B** and sits in a mailbox queue. 
5. **Client B** connects to **Server B** using `IMAP` (or `POP3`) to pull the email down to read it.

---

### Common HLD Interview Questions

**Q1: When designing an email client, when would you use IMAP instead of POP3?**
*Answer:* You use IMAP when users need to sync across multiple devices.
*Example:* Deleting an email on your iPhone (IMAP) marks it as deleted on the server, so you never see it again when you open your laptop later. POP3 would download disparate local copies.

**Q2: Why do we need two different protocols (SMTP and IMAP/POP3) to handle email?**
*Answer:* They serve opposing functional roles: pushing vs pulling.
*Example:* You use SMTP to forcefully "push" an outgoing email to Google's servers. You use IMAP to "pull" your inbox contents back to your phone to read them.
