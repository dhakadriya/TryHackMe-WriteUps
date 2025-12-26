# 🧾 TryHackMe Write-Up  
## Forensics — Registry Furensics  
*(Advent of Cyber 2025 — Day 16)*

---

## 📌 Overview

This room introduces **Windows Registry Forensics** — analysing offline registry
hives to uncover evidence of system activity, persistence mechanisms, and user
behaviour. Using **Registry Explorer**, we investigate the compromised system
`dispatch-srv01` to identify suspicious application execution and persistence
linked to the TBFC intrusion.

The exercise reinforces:

- Windows Registry structure & hive locations  
- Understanding forensic-relevant registry keys  
- Offline registry analysis workflow  
- Tracking program execution & persistence behaviour  

---

## 🧩 Room Information

| Field | Details |
|------|--------|
| Platform | TryHackMe |
| Room | Forensics — Registry Furensics |
| Event | Advent of Cyber 2025 |
| Category | DFIR / Windows Forensics |
| Difficulty | Beginner–Intermediate |
| Estimated Time | ~60 minutes |
| Status | Completed (100%) |

---

## 🧠 Windows Registry — Key Concepts

The Windows Registry is the OS “brain,” storing configuration such as:

- User activity & preferences  
- Installed applications  
- Autostart programs  
- Device history  
- System identity & configuration  

Registry data is stored across multiple **hives**, including:

- `SYSTEM`
- `SOFTWARE`
- `SAM`
- `SECURITY`
- `NTUSER.DAT`
- `USRCLASS.DAT`

During incident response, these hives are analysed **offline** to avoid altering
evidence.

---

## 🛠️ Tools Used

- **Registry Explorer** (Eric Zimmerman)
  - Supports offline hive loading
  - Replays transaction logs
  - Parses binary-encoded registry values
  - Provides forensic bookmarks & searching

---

## 🔍 Investigation Scenario

- Host under investigation: **dispatch-srv01**
- Suspicious activity date: **21 Oct 2025**
- Registry hives collected and analysed offline from:

C:\Users\Administrator\Desktop\Registry Hives

Goal: Identify the application tied to malicious activity and determine how it
was executed and persisted.

---

## 🧪 Analysis Walkthrough

### 🟡 Step 1 — Load Offline Registry Hives

1. Open **Registry Explorer**
2. Select **File → Load Hive**
3. Choose hive file (ex: `SYSTEM`, `SOFTWARE`, `NTUSER.DAT`)
4. Hold **SHIFT + Open** to replay transaction logs
5. Repeat for remaining hives

✔ Ensures a clean, consistent forensic copy

---

### 🟡 Step 2 — Identify Host Information

Navigate:

ROOT\ControlSet001\Control\ComputerName\ComputerName


Hostname confirmed:

DISPATCH-SRV01


---

### 🟡 Step 3 — Identify Installed / Executed Application

Using forensic bookmarks & key paths, evidence revealed an installed program:

DroneManager Updater

➡️ **Answer:** `DroneManager Updater`

Execution path located via **RunMRU / RecentDocs / App Paths** artefacts:

C:\Users\dispatch.admin\Downloads\DroneManager_Setup.exe

➡️ **Answer:**  
`C:\Users\dispatch.admin\Downloads\DroneManager_Setup.exe`

---

### 🟡 Step 4 — Identify Persistence Mechanism

Persistence discovered under startup entries:

HKLM\Software\Microsoft\Windows\CurrentVersion\Run


Value added:

"C:\Program Files\DroneManager\dronehelper.exe" --background


➡️ **Answer:**  
`"C:\Program Files\DroneManager\dronehelper.exe" --background`

This confirms the application **auto-launched on user login**, a common attacker
persistence tactic.

---

## 🚩 Answers Collected

| Question | Answer |
|--------|--------|
| Installed application prior to intrusion | `DroneManager Updater` |
| Full path where app was launched from | `C:\Users\dispatch.admin\Downloads\DroneManager_Setup.exe` |
| Persistence value added at startup | `"C:\Program Files\DroneManager\dronehelper.exe" --background` |

---

## 🧠 Key Learnings

- Registry artefacts reveal:
  - Program execution history  
  - User activity trails  
  - Startup persistence entries  
- Offline hive analysis prevents evidence tampering  
- DFIR requires correlating multiple registry sources  
- Transaction log replay is essential for data integrity  

---

## 🔐 Forensic Takeaways

- Monitor **Run / RunOnce / Startup** registry keys
- Track executables launched from **Downloads** directories
- Persistence entries often indicate **post-compromise activity**
- Registry Explorer is a core tool for Windows DFIR investigations

---

## ✅ Conclusion

This room provides practical experience in **registry-based forensic analysis**,
demonstrating how investigators extract evidence from offline hives to identify
malicious execution and persistence mechanisms on compromised Windows systems.

---

## 🔗 References

- TryHackMe — *Registry Furensics (Advent of Cyber 2025)*

---
