# Android7_Forensic_Analysis (Autopsy)
A hands-on forensic analysis using Autopsy on Android 7 image


This project is a complete forensic walkthrough using Autopsy on an Android 7 system image. The goal was to simulate a real-world investigation and extract key data artifacts, deleted items, web accounts, and messaging records.

---
## 🎯 User Behavior & Device Profiling
Goal: To uncover insights into the user’s behavior patterns, communication style, app usage, and device configuration.
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

## 📱Messaging App Analysis & User Identification
Android Messages (SMS)

SMS Registration Confirmation – TracFone (User Number Identification)

![Screenshot](https://github.com/saifsk33/Android7_Forensic_Analysis/blob/main/images/Screenshot%20(30).png?raw=true)

🔍 Observation:
The first SMS received on the device is from short code 31778, linked to TracFone, a U.S. prepaid mobile provider. The message includes the user's assigned phone number.

📄 Evidence:

“Welcome to TracFone! Your number is: 9197580276, your Last Day of Service is 01/28/2019…”

📌 Significance:
This message confirms the user’s phone number as +1 919-758-0276, which can be used as a reference point when mapping other communications throughout the device.


## 🔐Encryption of user identity by Android Message system
![Screenshot](https://github.com/saifsk33/Android7_Forensic_Analysis/blob/main/images/Screenshot%20(29).png?raw=true)

🔍 Observation: 
Autopsy analysis shows that inbound SMS messages retain sender phone numbers in plain text (e.g., +19102697333), while the user’s own number is abstracted using a UUID format (e.g., 3e436738-93b7-4fb9-ab39-dfd6e2f5bcdf).

📄 Evidence:
>A sample SMS from +19102697333 reads: "That's fine. Sometimes SMS is better."

📌 Significance:
This pattern suggests that the Android Messages app internally encrypts or abstracts the user’s identity but not the sender’s. This helps investigators confirm communication origin while also tying a UUID to the device owner for further tracking.

## 📱 Viber Messaging – No Number Encryption

![Screenshot](https://github.com/saifsk33/Android7_Forensic_Analysis/blob/main/images/Screenshot%20(36).png?raw=true)

🔍 Observation:
Viber stores both the sender's and receiver's phone numbers in plain text, unlike native SMS apps which encrypt the user's number. A typical Viber message appears as:

📄 Evidence:

>From: +1 919-758-0276

>To: +1 910-269-7333

>Message: “Hey there. I have never heard of this app, but it’s pretty popular.”

📌 Significance:
The lack of encryption simplifies the process of tracing communication between specific parties. However, it also presents potential privacy vulnerabilities in Viber’s data storage practices.

## 📲 WhatsApp Messaging – Partial Number Obfuscation

![Screenshot](https://github.com/saifsk33/Android7_Forensic_Analysis/blob/main/images/Screenshot%20(31).png?raw=true)

Observation:
Messages extracted from WhatsApp reveal that the sender’s phone number is stored in plain text, as seen in the screenshot:

>From: 19102697333@s.whatsapp.net

>To: 3e436738-93b7-4fb9-ab39-dfd6e2f5bcdf

>Message: "Hey man! What’s happening? Welcome to WhatsApp!"



The recipient's number (user) is obfuscated using a UUID format, while the sender's identity remains visible.

Significance:
This pattern of storage is similar to the native Android Messaging app. It suggests WhatsApp encrypts or replaces the user’s number with a unique identifier (possibly for privacy), while allowing incoming message origins to be clearly traced. This can help in mapping contact networks but limits visibility into full two-way communication unless additional user ID mappings are found.

## 📲 IMO Messaging – Full Number Obfuscation

![Screenshot](https://github.com/saifsk33/Android7_Forensic_Analysis/blob/main/images/Screenshot%20(32).png?raw=true)

Observation:
In messages extracted from the IMO messaging app, both the sender and receiver identifiers appear as numerical codes, rather than actual phone numbers.

> From: 2002241318462661

>To: 2002144460145418

>Message: "He there! Welcome to IMO. About time you showed up."



Significance:
Unlike other messaging platforms analyzed (such as Android Messages, WhatsApp, and Viber), IMO does not store phone numbers in plain text. Instead, it likely uses internal user or device IDs for both sender and recipient. This may indicate a higher level of privacy protection or internal indexing.

This limits direct user identification unless these codes can be mapped to known numbers through other artifacts or app databases. Same goes for Facebook Messenger and Shareit.

## 📲Google App Cache – Weather & Content Personalization Evidence

![Screenshot](https://github.com/saifsk33/Android7_Forensic_Analysis/blob/main/images/Screenshot%20(56).png?raw=true)

📁 Source File Path:
/img_blk0_mmcb1k0.bin/vol_vol52/data/com.android.vending/cache/images/OI-9C6HsEyXIZZEgktnUxkOddoA

Observation: Analysis of a cache file linked to the Google App (or its Discover feed component) states strong indicators of user interaction with weather and news card features which reveals repeated references to "Holly Springs," a real geographic location.
Notable content includes:
- Repeated references to *Holly Springs* — a real geographic location.
 
- Weather descriptions such as: 
 
“Clear with periodic clouds” “Partly Cloudy” “Thursday” / “Friday”

- User interface phrases associated with Discover feed interaction:

“Hide this story”

“Change temperature units to C”

“Never show weather updates”

“Not interested in Donald Trump”

“Customize weather”

"Card category: WEATHER"

"Send feedback", "Story hiddenR", "Donald Trump", "Not interested in Donald Trump"

Significance:
These references strongly suggest the user searched for or viewed weather data related to Holly Springs, pointing to a potential user location or area of interest.
The political filtering entry (“Not interested in Donald Trump”) and feedback-related strings point to personalized content curation behavior. Together, this strengthens behavioral profiling and provides potential geographic context.


## 🧾 Conclusion: Messaging App Analysis & User Identification
The user's phone number was successfully uncovered through the Viber messaging app, which stored both sender and receiver information in plain text. The user's phone number (+1 919-758-0276) was further confirmed via an SMS from TracFone, a U.S.-based prepaid mobile provider, in a welcome message stating the assigned number.

Among the messaging apps analyzed, Android Messages, Viber, and WhatsApp all retained the sender's contact number in plain text, allowing investigators to identify communication partners and reconstruct chat patterns. Notably, incoming messages across these platforms consistently referenced the same number: +1 910-269-7333, stored as Josh Hickman in the device's contacts2.db.

In contrast, messaging platforms such as Facebook Messenger, IMO, and SHAREit stored both sender and receiver identifiers in obfuscated or numerical formats, offering limited traceability without deeper database correlation or cross-referencing.

📌 Key Insight:
Across multiple platforms, the user consistently communicated with the same contact — Josh Hickman — using different messaging apps. This strongly suggests that Josh Hickman was the user’s primary or only active contact during the device's period of use.

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

