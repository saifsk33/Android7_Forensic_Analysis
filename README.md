# Android7_Forensic_Analysis (Autopsy)
A hands-on forensic analysis using Autopsy on Android 7 image


This project is a complete forensic walkthrough using Autopsy on an Android 7 system image. The goal was to simulate a real-world investigation and extract key data artifacts, deleted items, web accounts, and messaging records.

---
## 🎯 User Behavior & Device Profiling
Goal: To uncover insights into the user’s behavior patterns, communication style, app usage, and device configuration.

## 🔍 Case Objectives

- Extract messages, contacts, and user account credentials.
- Recover deleted data (SMS, chat logs, Wi-Fi configs).
- Identify app activity, timestamps, and user behavior.
- Link device metadata with user identity and location.
- Validate findings using Autopsy, SQLite, and file system analysis.

---

## 🧠 Key Findings Summary

- ✅ **Recovered TikTok password stored in plaintext**
- ✅ **Recovered user phone number from SMS artifacts (Android Messages)**
- ✅ **Retrieved Wi-Fi SSIDs, BSSIDs, and passwords from deleted APK**
- ✅ **Recovered AT&T SIM details from naver_line.db**
- ✅ **Found Google cache content referencing Holly Springs, NC**
- ✅ **Observed system timezone set to IST despite U.S. usage**
- ✅ **Recovered contact lists from Viber, WhatsApp, and default contact apps**
- ✅ **Identified app credentials for Google, Signal, Telegram, IMO, etc.**
- ✅ **Recovered encrypted tokens and profile info from Line app**

---

## 📂 Artifacts Collected

| Artifact Type             | Tool Used            | Description                                                                                      |
|---------------------------|----------------------|--------------------------------------------------------------------------------------------------|
| **Messages**              | Autopsy, DB Browser  | SMS from Android Messages, conversations from WhatsApp, Viber and IMO      |
| **Contacts**              | Autopsy              | Extracted from `viber_data`, and other app-specific databases                    |
| **Web Accounts**          | Autopsy              | App credentials for TikTok (plaintext), Google, Telegram, Signal, and others                     |
| **Deleted Files**         | Autopsy              | Contained Wi-Fi configs, device identifiers, and data from uninstalled APKs                      |
| **Wi-Fi Configurations**  | Autopsy (Deleted APK)| SSIDs, BSSIDs, and passwords for multiple networks; useful for location correlation              |
| **SIM & Carrier Metadata**| DB Browser (SQLite)  | SIM Serial Number, SIM Operator Code (AT&T), Country Code, and Phone Verification Timestamp      |
| **Location Artifacts**    | Autopsy (Cache Files)| Google App cache with weather/location (Holly Springs), IST timestamp mismatch                   |
| **Encryption Evidence**   | Autopsy              | High-entropy files from apps like Signal and IMO, indicating encrypted storage                   |
| **Profile Metadata**      | DB Browser           | Line app profile keys, encrypted access tokens, country calling codes, and profile phone data    |


---

## 📡 **Recovered Deleted Wi-Fi Configuration Containing SSIDs, Passwords, and Device Info**

![Screenshot](https://github.com/saifsk33/Android7_Forensic_Analysis/blob/main/images/Screenshot%20(53).png?raw=true)

**🧠 What Was Found:**

A deleted file located at:

/misc/radio/modem_config/.../AdsDynamite.apk


Though partially removed, this file retained critical configuration data:

### 🔍 Device Details:

| Key           | Value          |
| ------------- | -------------- |
| Manufacturer  | LGE            |
| Model Name    | Nexus 5X       |
| Serial Number | 007289511f431f6d |
| Device Name   | bullhead       |


### 📶 Wi-Fi Network Artifacts:

Two saved Wi-Fi profiles were successfully recovered, including SSID, BSSID, and PSK:

#### 1️⃣ First Wi-Fi Network:

* **SSID**: E04201358294
* **BSSID**: 00:19:77:37:4d:54
* **Password (PSK)**: 2ThisFor@llIs#
* **Key Management**: WPA-PSK

#### 2️⃣ Second Wi-Fi Network:

* **SSID**: CcookiesDcastleR5
* **BSSID**: f8:bb:bb:1f:fa:e8
* **Password (PSK)**: SorryThereIsNoPasswordHere
* **Key Management**: WPA-PSK


#### 📌 Significance:

* The Wi-Fi credentials and hardware identifiers were found inside a **deleted APK configuration**, showing the data wasn’t completely erased.
* These SSIDs and passwords may help **establish geographic location**, **user movement**, or **connection timelines**.
* The serial number, device codename (bullhead), and model help **identify the physical device** involved.

## 📲Google App Cache – Weather & Content Personalization Evidence

![Screenshot](https://github.com/saifsk33/Android7_Forensic_Analysis/blob/main/images/Screenshot%20(56).png?raw=true)

#### 📁 Source File Path:
/img_blk0_mmcb1k0.bin/vol_vol52/data/com.android.vending/cache/images/OI-9C6HsEyXIZZEgktnUxkOddoA

#### 🔍 Observation: 
Analysis of a deleted cache file linked to the Google App (or its Discover feed component) states strong indicators of user interaction with weather and news card features which reveals repeated references to "Holly Springs," a real geographic location.

Notable content includes:
Repeated references to *Holly Springs* — a real geographic location.
 
User interface phrases associated with Discover feed interaction & Weather descriptions such as: 
 
“Clear with periodic clouds” “Partly Cloudy” “Thursday” / “Friday”

“Hide this story”

“Change temperature units to C”

“Never show weather updates”

“Not interested in Donald Trump”

“Customize weather”

"Card category: WEATHER"

"Send feedback", "Story hiddenR", "Donald Trump", "Not interested in Donald Trump"

#### 📌 Significance:
These references strongly suggest the user searched for or viewed weather data related to Holly Springs, pointing to a potential user location or area of interest.
The political filtering entry (“Not interested in Donald Trump”) and feedback-related strings point to personalized content curation behavior. Together, this strengthens behavioral profiling and provides potential geographic context.

Interestingly, the **system timestamp is set to IST (Indian Standard Time)**, while the cache content and weather data reference a **U.S. location**. This mismatch suggests that the user may **reside in Holly Springs** but manually set the device’s timezone to IST — a potential **indicator of deliberate obfuscation** or **cross-regional usage**. We did find SIM Operator Code in one of the db files which is called naver_line.db which confirms that the user is based in US and not India. 
 
Together, this strengthens both **behavioral profiling** and **geographic inference** in the forensic analysis.

## 📱 Naver Line App – Device and SIM Metadata Extracted

![Screenshot](https://github.com/saifsk33/Android7_Forensic_Analysis/blob/main/images/Screenshot%20(51).png?raw=true)
 
#### 🔍 Observation:
 
Using **DB Browser for SQLite**, the naver_line.db file was analyzed, specifically the setting table, to extract device, SIM, and network metadata. This database belongs to the LINE messaging app.
  
🔢 **SIM & Carrier Information**
 
- **SIM Serial Number**: 457438409
 
- **SIM Operator Code (MCC+MNC)**: 310410 
 
  - 310: Mobile Country Code for **United States**
 
  - 410: Mobile Network Code for **AT&T**
 
This confirms that the device was using an **AT&T SIM card** and was likely operating within the U.S.
  .
  .
  .
📞 **Phone & Profile Metadata**
 
 
- **Phone Verification Timestamp**: 1543758224480 (Unix Epoch – approx. Dec 2, 2018)
 
- **Country Calling Code**: +1 (United States / Canada)
 
- **Profile Phone**: ZCRdmEjoMnv+fhvksIZI0Q== (Base64 encoded or encrypted)
 
- **Profile Name**: jMlQW7nOoYFz+nJPzjG== (Possibly encoded username)
 
- **App Version**: 19081008_8.18.1
 
- **Wi-Fi Access Timestamp**: 1549351316155 (from USER_STATUS_ACCESS_WIFI_NETWORK_TIME)
 
  
🧩 **Tools Used:**

 - **Autopsy**: To extract the file 
 
- **SQLite Viewer**: DB Browser for SQLite
 
- **File Analyzed**: naver_line.db
 
  
#### 📌 Significance:
 
 
- The **AT&T SIM operator code** (310410) confirms usage of a U.S. carrier, supporting the case’s geolocation analysis.
 
- SIM serial number, phone verification timestamp, and country code help construct the user’s device identity and timeline of use.
 
- Encrypted or encoded profile fields might reveal account links if decrypted in future analysis.
 
- Foreground/background timestamps and Wi-Fi activity give insight into app usage behavior.

## 🔐 Web Accounts Artifact – App Credential Storage

![Screenshot](https://github.com/saifsk33/Android7_Forensic_Analysis/blob/main/images/Screenshot%20(41).png?raw=true)

#### 🔍 Observation:

Autopsy’s Web Accounts section reveals stored credentials and associated metadata for multiple apps and services. The evidence is sourced from blk0_mmcbkl0.bin, and includes accounts for:

Google

Twitter

WhatsApp

IMO

TextNow

TikTok

Facebook Messenger

Skype

Telegram

Signal


#### 🧩 Key Findings:

1. Plaintext Password Storage:

⚠️ TikTok stores the password in plain text, visible as TikTok under the Password column.

This poses a significant security risk and is a notable exception compared to most apps that hash or encrypt stored credentials.




2. Hashed/Obfuscated Passwords:

Apps like Google, Signal, and Telegram use hashed or token-style strings for their credentials (e.g., long alphanumeric sequences), suggesting a more secure approach.

Example: 61aa7c8d9a21ce60d852eaaf749d7069d5754fa (Signal)

Example: aas_et/AKpp... (Google Auth)




3. User Identifiers:

Multiple entries for the same services (e.g., WhatsApp, Google, IMO) indicate reuse across sessions or installations.

User IDs include both email addresses (thisisdfir@gmail.com) and phone numbers (+19197580276).



4. Program Names:

Application package identifiers are listed clearly, e.g.,:

com.zhiliaoapp.musically (TikTok)

com.securesms (Signal)

org.telegram.messenger (Telegram)

com.facebook.orca (Messenger)


#### 📌 Significance:

The presence of plaintext passwords (TikTok) poses a privacy vulnerability.

This artifact confirms active user accounts on at least 10 different apps and can be used to correlate messages and contact activity with specific services.



## 📱Messaging App Analysis & User Identification
Android Messages (SMS)

SMS Registration Confirmation – TracFone (User Number Identification)

![Screenshot](https://github.com/saifsk33/Android7_Forensic_Analysis/blob/main/images/Screenshot%20(30).png?raw=true)

#### 🔍 Observation:
The first SMS received on the device is from short code 31778, linked to TracFone, a U.S. prepaid mobile provider. The message includes the user's assigned phone number.

#### 📄 Evidence:

“Welcome to TracFone! Your number is: 9197580276, your Last Day of Service is 01/28/2019…”

#### 📌 Significance:
This message confirms the user’s phone number as +1 919-758-0276, which can be used as a reference point when mapping other communications throughout the device.


## 🔐Encryption of user identity by Android Message system
![Screenshot](https://github.com/saifsk33/Android7_Forensic_Analysis/blob/main/images/Screenshot%20(29).png?raw=true)

#### 🔍 Observation: 
Autopsy analysis shows that inbound SMS messages retain sender phone numbers in plain text (e.g., +19102697333), while the user’s own number is abstracted using a UUID format (e.g., 3e436738-93b7-4fb9-ab39-dfd6e2f5bcdf).

#### 📄 Evidence:
>A sample SMS from +19102697333 reads: "That's fine. Sometimes SMS is better."

#### 📌 Significance:
This pattern suggests that the Android Messages app internally encrypts or abstracts the user’s identity but not the sender’s. This helps investigators confirm communication origin while also tying a UUID to the device owner for further tracking.

## 📱 Viber Messaging – No Number Encryption

![Screenshot](https://github.com/saifsk33/Android7_Forensic_Analysis/blob/main/images/Screenshot%20(36).png?raw=true)

#### 🔍 Observation:
Viber stores both the sender's and receiver's phone numbers in plain text, unlike native SMS apps which encrypt the user's number. A typical Viber message appears as:

#### 📄 Evidence:

>From: +1 919-758-0276

>To: +1 910-269-7333

>Message: “Hey there. I have never heard of this app, but it’s pretty popular.”

#### 📌 Significance:
The lack of encryption simplifies the process of tracing communication between specific parties. However, it also presents potential privacy vulnerabilities in Viber’s data storage practices.

## 📲 WhatsApp Messaging – Partial Number Obfuscation

![Screenshot](https://github.com/saifsk33/Android7_Forensic_Analysis/blob/main/images/Screenshot%20(31).png?raw=true)

#### 🔍 Observation:
Messages extracted from WhatsApp reveal that the sender’s phone number is stored in plain text, as seen in the screenshot:

>From: 19102697333@s.whatsapp.net

>To: 3e436738-93b7-4fb9-ab39-dfd6e2f5bcdf

>Message: "Hey man! What’s happening? Welcome to WhatsApp!"



The recipient's number (user) is obfuscated using a UUID format, while the sender's identity remains visible.

#### 📌 Significance:
This pattern of storage is similar to the native Android Messaging app. It suggests WhatsApp encrypts or replaces the user’s number with a unique identifier (possibly for privacy), while allowing incoming message origins to be clearly traced. This can help in mapping contact networks but limits visibility into full two-way communication unless additional user ID mappings are found.

## 📲 IMO Messaging – Full Number Obfuscation

![Screenshot](https://github.com/saifsk33/Android7_Forensic_Analysis/blob/main/images/Screenshot%20(32).png?raw=true)

#### 🔍 Observation:
In messages extracted from the IMO messaging app, both the sender and receiver identifiers appear as numerical codes, rather than actual phone numbers.

> From: 2002241318462661

>To: 2002144460145418

>Message: "He there! Welcome to IMO. About time you showed up."



#### 📌 Significance:
Unlike other messaging platforms analyzed (such as Android Messages, WhatsApp, and Viber), IMO does not store phone numbers in plain text. Instead, it likely uses internal user or device IDs for both sender and recipient. This may indicate a higher level of privacy protection or internal indexing.

This limits direct user identification unless these codes can be mapped to known numbers through other artifacts or app databases. Same goes for Facebook Messenger and Shareit.




Here’s a well-written **final conclusion** section you can paste at the end of your GitHub forensic report. It wraps up your findings professionally and highlights all the critical artifacts recovered during analysis:

---
  
## 🧾 Final Conclusion
 
Through methodical forensic analysis of the Android 7 image using **Autopsy** and **DB Browser for SQLite**, we successfully uncovered a wide range of user-identifiable data and device-specific insights. These findings offer strong evidence of both user activity and geographic behavior.
 
### 📍 **User and Device Identification Highlights:**
 
 
- **Device Manufacturer & Model:** Identified as **LGE Nexus 5X** (codename: *bullhead*), with unique serial number `0295111f431f6d`.
 
- **SIM Information:** SIM Operator Code `310410` confirms **AT&T (U.S.)** carrier. SIM serial number and other values further strengthen the U.S.-based usage.
 
- **User Phone Number:** Recovered from Android Messages (SMS) — **+1 919-758-0276** — giving us a direct line of identification.
 
- **Wi-Fi Credentials:** Recovered SSIDs, BSSIDs, and passwords provide real-world geolocation clues and could link the device to specific physical locations.
 

 
### 🌍 **Behavioral and Location Analysis:**
 
 
- **Google App Cache:** Revealed repeated weather data for **Holly Springs**, strongly suggesting the user either resides in or frequently monitors that location.
 
- The system timezone was set to **IST (Indian Standard Time)**, despite U.S.-based indicators, which could imply **deliberate obfuscation** or **cross-border behavior**.
 
- Google Discover-like artifacts showed **personalized content interaction**, including filtering out political content (e.g., “Not interested in Donald Trump”).
 

 
### 🔐 **Application & Communication Insights:**
 
 
- **Credential Recovery:** Plaintext passwords from TikTok and credential traces from apps like Google, Telegram, and WhatsApp reveal significant privacy exposure.
 
- **App Messaging Behavior:** 
 
  - **Viber Messaging App** stored user number plainly.
 
  - **WhatsApp**, **Viber** and **Android Messaging (SMS)** stored numbers with partial or no encryption.
 
  - **IMO**, **Facebook Messenger**, and **ShareIt** showed full number obfuscation, offering more privacy but requiring correlation from other data sources.
 
- **Line App Metadata:** Helped pinpoint timestamps of app usage, phone verification, and encoded profile fields that may aid further identity tracing.
 

  
### 🧠 Summary:
 
This investigation reconstructed a detailed digital footprint of the device's owner — including **location behavior**, **personal accounts**, **communication history**, **Wi-Fi access**, and **device configurations**.
 
The combination of **plaintext credentials**, **identified accounts**, **geographic artifacts**, and **SIM/operator evidence** paints a high-confidence picture of the user’s identity and behavior. These findings not only support attribution and timeline construction but also demonstrate how powerful Android forensics can be in real-world investigations.
  

---

## 🧰 Tools Used

- **Autopsy** (Windows VM)
- **DB Browser for SQLite**
- **Windows VM & Kali for dual OS setup**

---

## 🧑‍💻 Author

**KevSec**  
Cybersecurity & Digital Forensics Enthusiast


