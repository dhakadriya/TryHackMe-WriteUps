# 🎣 TryHackMe Write-Up  
## Phishing – Merry Clickmas  
*(Advent of Cyber 2025 – Day 02)*

---

## 📌 Overview

This room is part of **TryHackMe Advent of Cyber 2025** and focuses on
**phishing attacks using social engineering techniques**.  
The objective is to understand how attackers craft convincing phishing emails,
host fake login pages, capture user credentials, and analyze the real-world
impact of such attacks.

The exercise demonstrates how **human factors** are often the weakest link in
security.

---

## 🧩 Room Information

| Field | Details |
|------|--------|
| Platform | TryHackMe |
| Room Name | Phishing – Merry Clickmas |
| Event | Advent of Cyber 2025 |
| Category | Social Engineering / Phishing |
| Difficulty | Beginner |
| Estimated Time | ~30 minutes |
| Status | Completed (100%) |

---

## 🎯 Objectives

- Understand phishing as a form of social engineering
- Host a fake login page to capture credentials
- Send phishing emails using the Social-Engineer Toolkit (SET)
- Harvest credentials entered by a victim
- Assess the impact of credential reuse

---

## 🛠 Tools Used

- **AttackBox**
- **Social-Engineer Toolkit (SET)**
- **Python phishing server (server.py)**
- **Web Browser**
- **Linux Terminal**

---

## 🧠 Key Concepts Covered

- Social Engineering
- Phishing & Credential Harvesting
- Email Spoofing & Trust Exploitation
- Client-side credential capture
- Password reuse risks

---

## 🔍 Walkthrough

---

## 🧪 Task 1: Understanding Social Engineering & Phishing

The room begins by explaining **social engineering**, where attackers manipulate
human psychology using:
- Urgency
- Curiosity
- Authority
- Familiarity

Phishing is introduced as a common delivery method via:
- Emails
- SMS (Smishing)
- Calls (Vishing)
- QR codes (Quishing)

---

## 🪤 Task 2: Building the Phishing Trap

A **fake TBFC portal login page** was provided along with a Python script
(`server.py`) that:
- Hosts a phishing login page
- Captures submitted usernames and passwords
- Displays harvested credentials in real time

### Starting the phishing server

cd ~/Rooms/AoC2025/Day02
./server.py
The server listens on:

http://0.0.0.0:8000


This URL represents the phishing page that victims will interact with.

✉️ Task 3: Sending the Phishing Email Using SET

To send a convincing phishing email, the Social-Engineer Toolkit (SET) was
used.

Launching SET
setoolkit

Selected options:

Social-Engineering Attacks

Mass Mailer Attack

E-Mail Attack Single Email Address

Use own server or open relay

Email configuration:

Target Email: factory@wareville.thm

From Address: updates@flyingdeer.thm

From Name: Flying Deer

SMTP Server: MACHINE_IP

Port: 25

Subject: Shipping Schedule Changes

Email body included:

A convincing message

A link to the phishing page:

http://CONNECTION_IP:8000


The email was successfully delivered to the target user.

🧾 Task 4: Capturing Credentials

After a short delay, the victim interacted with the phishing page and entered
their credentials.

The credentials appeared directly in the terminal running server.py.

Retrieved password:
unranked-wisdom-anthem


This confirmed that the phishing attack was successful.

📦 Task 5: Credential Reuse Impact

Using the harvested credentials, access to the email portal was tested.

This revealed sensitive operational data, including:

Total number of toys expected for delivery:
1984000


This demonstrates how password reuse can significantly increase the impact
of a successful phishing attack.

🚩 Flags Collected
Question	Answer
Portal password	unranked-wisdom-anthem
Total toys	1984000
🧠 Key Learnings

Phishing relies more on psychology than technology

Convincing emails dramatically increase success rates

Hosting fake login pages is trivial for attackers

Credential reuse multiplies the damage of a breach

User awareness is critical for phishing prevention

🔐 Security Takeaway

Even well-secured systems can be compromised if users fall for phishing.
Regular security awareness training and multi-factor authentication (MFA)
are essential defenses.

✅ Conclusion

This room provided hands-on experience with phishing attack execution and
highlighted the severe risks posed by social engineering. It reinforced the
importance of user education, credential hygiene, and layered defenses in
modern cybersecurity.

🔗 References

TryHackMe Room:
https://tryhackme.com/room/phishingmerryclickmas
