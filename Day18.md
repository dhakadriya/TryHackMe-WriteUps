# 🥚 TryHackMe Write-Up  
## Obfuscation — The Egg Shell File  
*(Advent of Cyber 2025 — Day 18)*

---

## 📌 Overview

This room focuses on **obfuscation and de-obfuscation techniques** commonly used
by attackers to hide malicious content and evade detection. Through hands-on
exercises and analysis of the `SantaStealer.ps1` script, we explore how layered
encoding, XOR manipulation, and reversible transformations are used to conceal
C2 URLs, API keys, and script functionality.

The challenge simulates a DFIR workflow where analysts must decode and interpret
malicious behaviour before applying counter-obfuscation techniques.

---

## 🧩 Room Information

| Field | Details |
|------|--------|
| Platform | TryHackMe |
| Room Name | Obfuscation — The Egg Shell File |
| Event | Advent of Cyber 2025 |
| Category | Malware / Defensive Security |
| Difficulty | Beginner–Intermediate |
| Estimated Time | ~30 minutes |
| Status | Completed (100%) |

---

## 🧠 Key Concepts

### 🔹 What is Obfuscation?

Obfuscation is the process of **making code or data intentionally difficult to read**  
while keeping it functionally valid. Attackers use it to:

- Evade static detection
- Delay analyst understanding
- Hide commands and C2 URLs
- Bypass signature-based tools

---

### 🔹 Common Obfuscation Techniques

| Technique | Description | Indicators |
|---------|------------|-----------|
| ROT ciphers (ROT1 / ROT13) | Alphabet shifting | Words look “shifted” but readable |
| Base64 | Encodes data textually | Ends with `=` or `==` |
| XOR encoding | Bytes altered with a key | Same-length garbled output |
| Layered encoding | Multiple encoders stacked | Requires step-by-step decoding |

CyberChef is commonly used to **decode and analyze obfuscation layers**.

---

## 🛠 Tools Used

- 🧩 **CyberChef** — decoding & reversing transformations  
- 💻 **PowerShell** — executing and modifying script logic  
- 🕵️ Manual pattern analysis  

---

## 🔍 Challenge Walkthrough

---

## 🧪 Step 1 — Analyse and De-obfuscate C2 URL

The `SantaStealer.ps1` script contains an obfuscated C2 address in the
**“Start here”** section.

Steps performed:

1. Identify encoded string
2. Reverse the applied encoding operations
3. Restore readable C2 URL
4. Save script changes
5. Execute via PowerShell:
cd .\Desktop\
.\SantaStealer.ps1
🎉 Script execution revealed the first challenge flag.

➡️ Flag 1:
THM{C2_De0bfuscation_29838}

🔐 Step 2 — Obfuscate the API Key (XOR Encryption)
Part 2 required re-obfuscating the attacker’s API key using XOR.

Steps:

Locate API key constant in the script

Apply XOR encoding (via CyberChef or script logic)

Replace plaintext key with obfuscated value

Re-run script to validate behaviour

.\SantaStealer.ps1
🎉 Successful execution returned the second challenge flag.

➡️ Flag 2:
THM{API_Obfusc4tion_ftw_0283}

🚩 Flags Collected
Stage	Flag
After decoding C2 URL	THM{C2_De0bfuscation_29838}
After XOR-obfuscating API key	THM{API_Obfusc4tion_ftw_0283}

🧠 Key Learnings
Obfuscation is designed to delay and mislead defenders

ROT, Base64, XOR, and layered encodings remain widely used

CyberChef is invaluable for rapid decoding workflows

De-obfuscation requires recognising transformation patterns

Defenders must understand both:

how attackers hide payloads

and how to reverse those techniques

🔐 Defensive Insights
Monitor scripts for:

encoded strings

XOR operations

repeated decoding function calls

Treat unknown encoded payloads as suspicious indicators

Enforce sandbox execution before analysis

Educate SOC teams on common obfuscation patterns

✅ Conclusion
This room delivers practical experience in identifying and reversing script
obfuscation while demonstrating how attackers conceal malicious operations.
Understanding these techniques is essential for malware analysis, SOC triage,
and incident response investigations.

🔗 References
TryHackMe — The Egg Shell File (Obfuscation — AoC 2025)

