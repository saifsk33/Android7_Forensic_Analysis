# Android7_Forensic_Analysis (Autopsy)
A hands-on forensic analysis using Autopsy on Android 7 image


This project is a complete forensic walkthrough using Autopsy on an Android 7 system image. The goal was to simulate a real-world investigation and extract key data artifacts, deleted items, web accounts, and messaging records.

---

## 🔍 Case Objectives

- Extract messages, contacts, and web credentials.
- Recover deleted data (files, messages, images).
- Identify app activity, timestamps, and user behavior.
- Validate findings using SQLite and JSON formatting.

---

## 🧠 Key Findings Summary

- ✅ **Recovered TikTok password stored in plaintext**
- ✅ **Recovered missing messages by opening `threads_db2.db` in SQLite**
- ✅ **Extracted video metadata from Facebook messages using JSON formatter**
- ✅ **Verified deleted network config file and interfaces (`rt_tables`)**
- ✅ **Recovered contact lists from Viber, WhatsApp, and default contact apps**
- ✅ **Logged suspicious boot activity (`bootstat/boot_complete`)**
- ✅ **Detected encrypted files (`signal.db`, `modem.b20`, etc.)**

---

## 📂 Artifacts Collected

| Artifact Type        | Tool Used        | Description |
|----------------------|------------------|-------------|
| Messages             | Autopsy, SQLite  | Messages from Facebook, Android Message, WhatsApp |
| Contacts             | Autopsy          | Found in `contacts2.db`, `viber_data`, etc. |
| Web Accounts         | Autopsy          | TikTok, WhatsApp, Signal, TextNow |
| Deleted Files        | Autopsy          | Network configs, misc text/XML files |
| Media Attachments    | SQLite + JSON    | Facebook video/image attachments |
| Boot Logs            | Autopsy          | Found in `/data/misc/bootstat/` |
| Encryption Evidence  | Autopsy          | High-entropy encrypted files (Signal, etc.) |

---

## 📱 Messages
Android Messages (SMS)

SMS Registration Confirmation – TracFone (User Number Identification)



🔍 Observation:
The first SMS received on the device is from short code 31778, linked to TracFone, a U.S. prepaid mobile provider. The message includes the user's assigned phone number.

📄 Evidence:

“Welcome to TracFone! Your number is: 9197580276, your Last Day of Service is 01/28/2019…”

📌 Significance:
This message confirms the user’s phone number as +1 919-758-0276, which can be used as a reference point when mapping other communications throughout the device.

![Screenshot](https://raw.githubusercontent.com/saifsk33/Android7_Forensic_Analysis/2702b859865ba6afe71b89ae865a9985999f8593/images/Screenshot%20(29).png)

🔍 Observation: 
Autopsy analysis shows that inbound SMS messages retain sender phone numbers in plain text (e.g., +19102697333), while the user’s own number is abstracted using a UUID format (e.g., 3e436738-93b7-4fb9-ab39-dfd6e2f5bcdf).

📄 Evidence:
A sample SMS from +19102697333 reads:

"That's fine. Sometimes SMS is better."

📌 Significance:
This pattern suggests that the Android Messages app internally encrypts or abstracts the user’s identity but not the sender’s. This helps investigators confirm communication origin while also tying a UUID to the device owner for further tracking.


---

## 🧰 Tools Used

- **Autopsy** (Windows VM)
- **DB Browser for SQLite**
- **JSON Formatter (online)**
- **Windows VM & Kali for dual OS setup**

---

## ✍️ Report Notes

_(Paste the notes you made during the analysis here in bullet format – e.g., suspicious messages, timestamps, what was skipped, etc.)_

---

## 📎 Attachments

- [ ] `threads_db2.db` (sanitized)
- [ ] Recovered screenshots
- [ ] Extracted text/logs (optional)

---

## 🧑‍💻 Author

**Kevin Naka**  
Cybersecurity & Digital Forensics Enthusiast

---

