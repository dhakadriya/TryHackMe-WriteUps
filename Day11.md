# 🎄 TryHackMe Write-Up  
## XSS – Merry XSSMas  
*(Advent of Cyber 2025 – Day 11)*

---

## 📌 Overview

This room focuses on **Cross-Site Scripting (XSS)**, a common web vulnerability
that allows attackers to inject and execute malicious JavaScript inside a
victim’s browser.

The challenge demonstrates both:

- **Reflected XSS**
- **Stored XSS**

and explains how insecure handling of user input can lead to credential theft,
account takeover, and session hijacking.

---

## 🧩 Room Information

| Field | Details |
|------|--------|
| Platform | TryHackMe |
| Room Name | XSS – Merry XSSMas |
| Event | Advent of Cyber 2025 |
| Category | Web Security / XSS |
| Difficulty | Beginner |
| Estimated Time | ~30 minutes |
| Status | Completed (100%) |

---

## 🎯 Objectives

- Understand **Reflected vs Stored XSS**
- Inject malicious payloads through input fields
- Observe page behaviour and system logs
- Validate successful XSS execution
- Learn mitigation strategies

---

## 🧠 XSS Fundamentals

### 🔹 What is XSS?

A vulnerability where **untrusted user input is treated as code**, allowing
JavaScript execution in another user’s browser.

Attackers may:

- Steal session cookies
- Impersonate users
- Deface pages
- Trigger fake login prompts

---

### 🔹 Reflected XSS

- Payload is returned **immediately in the response**
- Usually delivered via **URL or form parameter**
- Commonly exploited via phishing links

Example:

<script>alert('Reflected Meow Meow')</script>
🔹 Stored XSS
Payload is saved on the backend

Executes for every user who loads the page

Far more dangerous

Example (stored inside a message/comment):

<script>alert('Stored Meow Meow')</script>
🛠 Tools & Techniques
Browser input fields

Burp/Logs for behaviour observation (optional)

System logs panel in the web app

Manual payload testing

🔍 Walkthrough
🧪 Step 1 — Testing Reflected XSS (Search Field)
Payload used:

<script>alert('Reflected Meow Meow')</script>
Action:

Enter payload in Search Messages

Click Search

Outcome:

Alert box appears → Reflected XSS confirmed

Logs show user input being reflected in output

📌 The input was not encoded or sanitised.

🏗 Step 2 — Testing Stored XSS (Message Form)
Same payload submitted as a message:

<script>alert('Stored Meow Meow')</script>
Action:

Submit message

Refresh or revisit page

Outcome:

Alert appears every time page loads

Confirms persistent / stored XSS

📌 The payload is stored in backend database.

🛡 Prevention & Mitigation
Key defensive practices:

Escape / encode untrusted input

Use textContent instead of innerHTML

Apply server-side validation and output encoding

Set cookies with:

HttpOnly

Secure

SameSite

Use HTML sanitisation libraries for rich-text inputs

🚩 Flags Collected
Challenge	Flag
Type of XSS that requires persistence	stored
Reflected XSS flag	THM{Evil_Bunny}
Stored XSS flag	THM{Evil_Stored_Egg}

🧠 Key Learnings
XSS occurs when user input is treated as executable code

Reflected XSS = immediate response execution

Stored XSS = persistent, affects all users

Poor input/output handling enables attacks

Defence requires sanitisation, encoding, and secure cookie policies

🔐 Security Takeaway
If user input is rendered without sanitisation, the browser will execute it —
and the attacker gains the same privileges as the victim session.

✅ Conclusion
This room provided hands-on experience identifying and exploiting Reflected
and Stored XSS vulnerabilities, emphasising the importance of secure input
handling and output encoding in web applications.

🔗 References
TryHackMe Room — Merry XSSMas
https://tryhackme.com/room/merryxssmas
