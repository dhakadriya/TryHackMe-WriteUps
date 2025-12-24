# 🎁 TryHackMe Write-Up  
## IDOR – Santa’s Little IDOR  
*(Advent of Cyber 2025 – Day 05)*

---

## 📌 Overview

This room is part of **TryHackMe Advent of Cyber 2025** and focuses on
**IDOR (Insecure Direct Object Reference)** vulnerabilities, a common form of
**access control and authorization bypass** in web applications.

The room demonstrates how attackers can access or manipulate data belonging to
other users by modifying object references such as IDs, encoded values, hashes,
or predictable tokens.

---

## 🧩 Room Information

| Field | Details |
|------|--------|
| Platform | TryHackMe |
| Room Name | IDOR – Santa’s Little IDOR |
| Event | Advent of Cyber 2025 |
| Category | Web Security / Access Control |
| Difficulty | Beginner |
| Estimated Time | ~45 minutes |
| Status | Completed (100%) |

---

## 🎯 Objectives

- Understand what IDOR vulnerabilities are
- Learn the difference between authentication and authorization
- Identify horizontal privilege escalation
- Exploit IDOR using numeric, encoded, hashed, and algorithmic references
- Understand secure design principles to prevent IDOR

---

## 🧠 IDOR Fundamentals

### 🔹 What is IDOR?

**IDOR (Insecure Direct Object Reference)** occurs when a web application:
- Uses user-supplied input to access objects (e.g., IDs)
- Fails to verify whether the authenticated user is authorized to access them

This often leads to **unauthorized data disclosure or modification**.

---

### 🔹 Authentication vs Authorization

- **Authentication** → Who are you?
- **Authorization** → What are you allowed to access?

IDOR vulnerabilities occur when **authorization checks are missing or broken**.

---

### 🔹 Privilege Escalation

- **Horizontal Privilege Escalation**  
  Accessing data belonging to another user at the same privilege level  
  👉 *Most IDOR vulnerabilities fall into this category*

- **Vertical Privilege Escalation**  
  Accessing features intended for higher-privileged users (e.g., admin)

---

## 🔍 Practical Exploitation Walkthrough

---

## 🧪 Task 1: Numeric IDOR (Obvious Reference)

After logging into the application with provided credentials, browser
**Developer Tools** were used to inspect network requests.

A request containing:

user_id=10

By modifying the `user_id` value in **Local Storage** to another number
(e.g., `11`) and refreshing the page, data belonging to another user became
visible.

📌 This confirms a **basic numeric IDOR vulnerability**.

---

## 🕵️ Task 2: Encoded References (Base64)

Some object references were encoded using **Base64**, for example:

Mg==

Decoding revealed it represented the number `2`.

Since encoding is reversible, modifying the decoded value and re-encoding it
allowed further IDOR exploitation.

📌 Encoding does **not** prevent IDOR.

---

## 🔐 Task 3: Hashed References (MD5)

Certain endpoints used hashed identifiers (e.g., MD5 hashes).

Although hashes look random, they are **deterministic**.  
If the original input is known or predictable, the hash can be recreated,
allowing attackers to enumerate and access unauthorized objects.

📌 Hashing alone is **not a security control**.

---

## 🎟 Task 4: Algorithmic IDOR (UUID v1)

Voucher codes appeared random but were identified as **UUID version 1**.

UUID v1 values are based on:
- Timestamp
- MAC address

Knowing the **time window of generation** allows attackers to brute-force valid
UUIDs and redeem vouchers they do not own.

📌 Predictable algorithms can introduce subtle IDOR risks.

---

## 🛡 Mitigation & Secure Design

To prevent IDOR vulnerabilities:

- Always perform **server-side authorization checks**
- Verify object ownership on every request
- Do not rely on:
  - Sequential IDs
  - Encoding (Base64)
  - Hashing alone
- Use random identifiers **with authorization**
- Log and monitor unauthorized access attempts
- Test applications by attempting cross-user access

---

## 🚩 Answers Collected

| Question | Answer |
|--------|--------|
| What does IDOR stand for? | Insecure Direct Object Reference |
| Privilege escalation type | Horizontal |
| user_id of parent with 10 children | `15` |

---

## 🧠 Key Learnings

- IDOR is fundamentally an **authorization flaw**
- Hiding IDs does not equal securing them
- Client-side controls cannot be trusted
- Authorization must be enforced on every request
- IDOR vulnerabilities are extremely common in real-world applications

---

## 🔐 Security Takeaway

> If a user can change a reference and access data they do not own,
> authorization has failed — regardless of how the reference looks.

---

## ✅ Conclusion

This room provided hands-on exposure to multiple forms of **IDOR vulnerabilities**,
from obvious numeric references to subtle algorithm-based flaws. It reinforced
the importance of **proper authorization checks and secure backend design** in
web applications.

---

## 🔗 References

- TryHackMe Room:  
  https://tryhackme.com/room/idor
