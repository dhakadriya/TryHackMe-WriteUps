# 🧬 TryHackMe Write-Up  
## Malware Analysis – Egg-xecutable  
*(Advent of Cyber 2025 – Day 06)*

---

## 📌 Overview

This room introduces **malware analysis fundamentals** using a safe sandbox
environment. The objective is to analyse a sample executable (`HopHelper.exe`)
using both **static and dynamic analysis techniques**, identify indicators of
compromise, persistence mechanisms, and network behaviour.

The room reinforces one key rule:

> 🛡️ Never run suspicious files on a real system — always use a sandbox / VM.

---

## 🧩 Room Information

| Field | Details |
|------|--------|
| Platform | TryHackMe |
| Room Name | Malware Analysis – Egg-xecutable |
| Event | Advent of Cyber 2025 |
| Category | Malware Analysis / Blue Team |
| Difficulty | Beginner – Intermediate |
| Estimated Time | ~45 minutes |
| Status | Completed (100%) |

---

## 🧠 Malware Analysis Concepts

Malware analysis helps defenders understand:

- What the malware does  
- How it behaves on execution  
- How it communicates  
- What changes it makes to the system  

There are **two main approaches**:

### 🔹 Static Analysis
Analysing a file **without executing it**
- Extracting metadata
- Viewing strings
- Checking imports & resources
- Calculating hashes

### 🔹 Dynamic Analysis
Running the file in a sandbox to observe:
- Registry changes
- Process behaviour
- File creations
- Network communication

---

## 🧰 Tools Used

- **PeStudio** – Static analysis
- **Regshot** – Registry change tracking
- **Process Monitor (ProcMon)** – Process and network activity analysis
- **Windows Sandbox VM / TryHackMe Analyst Machine**

---

## 🧪 Static Analysis (PeStudio)

The sample `HopHelper.exe` was inspected using PeStudio.

### ✔ Findings

| Artifact | Result |
|--------|-------|
| SHA256 Hash | `F29C270068F865EF4A747E2683BFA07667BF64E768B38FBB9A2750A3D879CA33` |
| Embedded string flag | `THM{STRINGS_FOUND}` |

The strings also revealed **payload hints and internal behaviour clues**.

---

## 🔬 Dynamic Analysis – Registry Persistence (Regshot)

Two registry snapshots were compared:

1️⃣ Before executing the malware  
2️⃣ After executing `HopHelper.exe`

### ✔ Persistence Mechanism Identified

The executable **created a Run key** to maintain persistence:

HKU\S-1-5-21-1966530601-3185510712-10604624-1008
Software\Microsoft\Windows\CurrentVersion\Run\HopHelper

yaml
Copy code

📌 This ensures the malware runs automatically on startup.

---

## 🌐 Dynamic Analysis – Network Behaviour (ProcMon)

ProcMon was filtered for **TCP activity** while the malware was running.

### ✔ Result

| Behaviour | Observation |
|---------|-----------|
| Protocol Used | `http` |
| Action Type | Outbound C2-style communication |

The sample attempted to contact a **remote web panel** (command-and-control).

---

## 🚩 Flags Collected

| Task | Answer |
|------|-------|
| SHA256 hash of HopHelper.exe | `F29C270068F865EF4A747E2683BFA07667BF64E768B38FBB9A2750A3D879CA33` |
| Strings flag | `THM{STRINGS_FOUND}` |
| Persistence registry key | `HKU\S-1-5-21-1966530601-3185510712-10604624-1008\Software\Microsoft\Windows\CurrentVersion\Run\HopHelper` |
| Network protocol used | `http` |

---

## 🧠 Key Learnings

- Malware often establishes **persistence via registry Run keys**
- Static analysis reveals intelligence **without executing the file**
- Dynamic analysis confirms **real behaviour and system impact**
- Sandboxes and VMs are essential for malware research
- Network monitoring helps identify **C2 communications**

---

## 🔐 Security Takeaway

> Malware rarely hides its intentions completely — careful static and dynamic
> analysis allows defenders to uncover behaviour, persistence, and indicators
> of compromise.

---

## ✅ Conclusion

This room provided hands-on experience in:
- Identifying malware artefacts
- Observing execution behaviour
- Detecting persistence mechanisms
- Analysing network communication

It is an excellent entry-level introduction to **practical malware analysis
workflow and tools**.

---

## 🔗 References

- TryHackMe Room – Malware Analysis: Egg-xecutable  
  https://tryhackme.com/room/malwareanalysis

---
