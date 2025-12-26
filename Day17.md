# 🔐 TryHackMe Write-up  
## CyberChef – Hoperation Save McSkidy  
*(Advent of Cyber 2025 – Day 17)*

---

## 📌 Overview

This TryHackMe room is part of the **Advent of Cyber 2025** series and focuses on
**encoding/decoding techniques using CyberChef** combined with **client-side
JavaScript logic analysis**.

The challenge is structured as a **five-level security system**, where each level
represents a lock that must be unlocked sequentially by obtaining valid
credentials.

---

## 🧩 Room Information

| Field        | Details |
|-------------|---------|
| Platform     | TryHackMe |
| Room Name    | CyberChef – Hoperation Save McSkidy |
| Category     | Encoding / Decoding |
| Difficulty   | Easy – Medium |
| Event        | Advent of Cyber 2025 |
| Objective    | Unlock all five levels and free McSkidy |

---

## 🏰 Lock Structure

| Level | Lock Name |
|------|----------|
| 1 | Outer Gate |
| 2 | Outer Wall |
| 3 | Guard House |
| 4 | Inner Castle |
| 5 | Prison Tower |

Each lock requires a **username and password** to proceed.

---

## 🔍 Common Pattern Observed

Across all levels, the following pattern was identified:

- **Username** is provided by a chatbot in an **encoded format**
- Encoding type: **Base64**
- **Password** is generated using **client-side JavaScript logic**
- Browser Developer Tools are required to inspect and understand this logic
- CyberChef is used to decode the username at every level

---

## 🛠 Tools Used

- **CyberChef**
- **Browser Developer Tools (Inspect Element)**
- **TryHackMe AttackBox / VPN**
- **JavaScript logic analysis**

---

## 🔑 Level-wise Solution

---

### 🥇 Level 1 – Outer Gate

**Username:**
- Chatbot provided an encoded username
- Used **CyberChef → Base64 Decode** to retrieve plaintext username

**Password:**
- Inspected JavaScript authentication logic
- Identified simple validation condition
- Derived correct password from the logic

**Result:** Outer Gate unlocked

---

### 🥈 Level 2 – Outer Wall

**Username:**
- New Base64 encoded username from chatbot
- Decoded using CyberChef

**Password:**
- JavaScript file revealed string manipulation logic
- Reconstructed password accordingly

**Result:** Outer Wall unlocked

---

### 🥉 Level 3 – Guard House

**Username:**
- Base64 encoded, decoded via CyberChef

**Password:**
- JavaScript included conditional checks
- Manual code tracing helped derive valid password

**Result:** Guard House unlocked

---

### 🏰 Level 4 – Inner Castle

**Username:**
- Retrieved from chatbot and Base64 decoded

**Password:**
- JavaScript logic was slightly obfuscated
- Included variable operations and comparisons
- Password derived by understanding execution flow

**Result:** Inner Castle unlocked

---

### 🏯 Level 5 – Prison Tower

**Username:**
- Final encoded username decoded using CyberChef

**Password:**
- Most complex logic among all levels
- Required careful analysis of JavaScript functions
- Correct password derived after evaluating conditions

**Result:** Prison Tower unlocked and McSkidy freed 🎉

---

## 🧠 Key Learnings

- Difference between **encoding and encryption**
- Practical usage of **CyberChef for decoding tasks**
- Importance of **client-side code inspection**
- Why authentication logic should never be fully exposed on the client side
- Real-world relevance of JavaScript reverse engineering

---

## 🔐 Security Takeaway

> Relying on client-side JavaScript for authentication logic is insecure.
> Sensitive operations should always be handled on the server side to
> prevent credential exposure and bypass attacks.

---

## ✅ Conclusion

This room provided hands-on experience with **encoding/decoding techniques** and
**web application logic analysis**. By solving each level sequentially, it
demonstrated how minor weaknesses in implementation can lead to complete system
compromise.

---

### 📎 References
- TryHackMe Room: https://tryhackme.com/room/encoding-decoding-aoc2025-s1a4z7x0c3
- CyberChef: https://gchq.github.io/CyberChef/

---
