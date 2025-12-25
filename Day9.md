# 🔓 TryHackMe Write-Up  
## Passwords – A Cracking Christmas  
*(Advent of Cyber 2025 – Day 09)*

---

## 📌 Overview

This room introduces **password-based encryption cracking**, focusing on how
attackers recover weak passwords protecting files such as PDFs and ZIP archives.
Rather than breaking encryption itself, attackers attempt to **guess the
password** using wordlists, dictionary attacks, or mask-based brute force.

The practical exercise walks through recovering passwords from **encrypted PDF
and ZIP files**, using common cracking tools.

---

## 🧩 Room Information

| Field | Details |
|------|--------|
| Platform | TryHackMe |
| Room Name | Passwords – A Cracking Christmas |
| Event | Advent of Cyber 2025 |
| Category | Password Cracking / Forensics |
| Difficulty | Beginner |
| Estimated Time | ~30 minutes |
| Status | Completed (100%) |

---

## 🎯 Objectives

- Understand how attackers crack weak encrypted files  
- Perform **dictionary-based password cracking**  
- Use tools such as:
  - `pdfcrack`
  - `zip2john`
  - `john`
- Extract file contents after recovering passwords  
- Recognize indicators of password-cracking activity for detection

---

## 🧠 Key Concepts

### 🔹 Why attackers target passwords, not encryption

Modern encryption is strong — attackers instead attempt to:

- Guess the password protecting the file
- Use leaked wordlists (e.g., `rockyou.txt`)
- Perform **dictionary or mask-based attacks**

---

### 🔹 Dictionary Attacks

- Fast and effective
- Use known passwords and common patterns
- Often succeed against human-chosen passwords

---

### 🔹 Mask / Brute-Force Attacks

- Try structured combinations  
  Example mask: `?l?l?l?d?d`
  - 3 lowercase letters + 2 digits  
- Used when the password pattern is partially known

---

## 🛠 Tools Used

- `pdfcrack`
- `zip2john`
- `john` (John the Ripper)
- Wordlist: `rockyou.txt`

---

## 🔍 Practical Walkthrough

---

## 🧪 Step 1 — Identify File Types

file flag.pdf
file flag.zip
This determines which cracking tool to use.

🧾 Step 2 — Cracking the Encrypted PDF (Dictionary Attack)

pdfcrack -f flag.pdf -w /usr/share/wordlists/rockyou.txt
Once the correct password was found, the file contents were unlocked and the
flag was revealed:

THM{Cr4ck1ng_PDFs_1s_34$y}
🗂 Step 3 — Cracking the Encrypted ZIP File
Convert ZIP to hash format for John:

zip2john flag.zip > ziphash.txt
Run dictionary attack:

john --wordlist=/usr/share/wordlists/rockyou.txt ziphash.txt
John successfully recovered the password and the file was extracted, revealing:

THM{Cr4ck1n6_z1p$_1s_34$yyyy}


🛡 Detection & Telemetry Awareness
Although cracking is offline, analysts may detect activity via:

Process creation logs (e.g., john, hashcat, pdfcrack)

Repeated reads of encrypted files and wordlists

GPU usage spikes (for accelerated cracking)

Downloading large wordlists or cracking tools

Potfile creation (john.pot, hashcat.potfile)

Monitoring signals include:

Sysmon Event ID 1 (Windows)

Linux auditd & EDR process events

GPU telemetry (nvidia-smi)

🚩 Flags Collected
Task	Flag
Encrypted PDF flag	THM{Cr4ck1ng_PDFs_1s_34$y}
Encrypted ZIP flag	THM{Cr4ck1n6_z1p$_1s_34$yyyy}

🧠 Key Learnings
Attackers rarely break encryption directly — they attack the password

Weak or common passwords make encrypted files vulnerable

Dictionary attacks are fast and highly effective

Mask attacks reduce brute-force time when structure is known

Cracking activity leaves endpoint-level forensic traces

🔐 Security Takeaway
Strong, unique passwords and passphrases are essential — especially for
encrypted archives and sensitive documents. Weak passwords defeat strong
encryption.

✅ Conclusion
This room provided hands-on experience with recovering passwords from encrypted
files, reinforcing how attackers exploit weak password choices and how
defenders can detect cracking behavior in monitored environments.

🔗 References
TryHackMe Room — Passwords: A Cracking Christmas
https://tryhackme.com/room/passwordscrackingchristmas
