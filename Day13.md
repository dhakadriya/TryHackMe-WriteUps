# 🧩 TryHackMe Write-Up  
## YARA Rules — YARA Mean One!  
*(Advent of Cyber 2025 — Day 13)*

---

## 📌 Overview

This room introduces **YARA**, a powerful malware-hunting and threat-detection
tool used by defenders to identify malicious files based on **patterns,
signatures, strings, and behavioral indicators**.

Rather than relying solely on antivirus signatures, YARA enables analysts to:

- Write custom rules  
- Hunt for malware variants  
- Detect suspicious strings, byte patterns, and encoded content  
- Perform post-incident detection across systems

In this challenge, defenders use YARA to scan files for malicious traces and
recover a hidden message left by McSkidy.

---

## 🧩 Room Information

| Field | Details |
|------|--------|
| Platform | TryHackMe |
| Room Name | YARA Rules — YARA Mean One! |
| Event | Advent of Cyber 2025 |
| Category | Threat Hunting / Malware Detection |
| Difficulty | Beginner–Intermediate |
| Estimated Time | ~60 minutes |
| Status | Completed (100%) |

---

## 🧠 What is YARA?

YARA is used to **identify and classify malware** using rules that match:

- Text strings  
- Hexadecimal byte sequences  
- Regular expressions  
- Encoded or obfuscated data  
- File properties (size, metadata, offsets)

It is commonly used for:

- 🕵️ Threat hunting  
- 🧪 Malware research  
- 🧩 Memory forensics  
- 🔎 Post-incident IOC scanning  

---

## 🧱 Structure of a YARA Rule

A YARA rule typically contains three sections:

### 🔹 Metadata  
Describes rule purpose and author context

### 🔹 Strings  
Defines the patterns to search for

### 🔹 Condition  
Specifies when the rule should trigger

Example:

rule TBFC_KingMalhare_Trace {
    meta:
        author = "Defender of SOC-mas"
        description = "Detects traces of King Malhare’s malware"

    strings:
        $s1 = "rundll32.exe" fullword ascii
        $url = /http:\/\/.*malhare.*/ nocase

    condition:
        any of them
}
🔍 Types of Strings in YARA
🟡 Text Strings (ASCII / Unicode)
Can include modifiers: wide, ascii, nocase

🟡 Encoded / Obfuscated Matching
xor, base64, base64wide

🟡 Hexadecimal Strings
Match raw byte patterns in binaries

🟡 Regular Expressions
Detect variable-pattern threats such as URLs or encoded payloads

⚙️ Useful YARA Scan Flags
Flag	Purpose
-r	Recursive directory scanning
-s	Show matched strings
man yara	Help and rule syntax reference

Example recursive scan:

yara -r rulefile.yar /path/to/scan
🧪 Practical Challenge
Goal: Search the directory
/home/ubuntu/Downloads/easter
for strings starting with:

TBFC:<alphanumeric>
Extract the hidden message from files that contain it.

🚩 Answers Collected
Question	Answer
How many images contain the string TBFC?	5
Regex used to match pattern	/TBFC:[A-Za-z0-9]+/
Message left by McSkidy	Find me in HopSec Island

🧠 Key Learnings
YARA enables custom malware detection beyond signatures

Strings may exist in:

plain text

hex bytes

encoded payloads

Conditions allow flexible logic (any, all, boolean operations)

YARA is essential for:

threat hunting

incident response

IOC validation

malware triage

🔐 Security Takeaway
YARA empowers defenders to move from reactive detection to proactive
threat hunting, enabling analysts to identify malware patterns — even when
attackers attempt obfuscation or code mutation.

🔗 References
TryHackMe — YARA Mean One! (Advent of Cyber 2025)
