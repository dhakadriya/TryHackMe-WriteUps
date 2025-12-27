# 🛰️ TryHackMe Write-Up  
## ICS & Modbus — Operation: North Pole Network  
*(Advent of Cyber 2025 — Day 19)*

---

## 📌 Overview

This challenge focuses on **Industrial Control Systems (ICS)** and the Modbus
protocol, demonstrating how attackers may interact with Operational Technology
(OT) networks. We investigate a simulated ICS environment to analyse network
behaviour, understand Modbus communication, and identify security weaknesses.

The goal is to recognise how ICS differs from IT environments and why OT
security requires caution, reliability, and visibility — especially in critical
infrastructure environments.

---

## 🧩 Room Information

| Field | Details |
|------|--------|
| Platform | TryHackMe |
| Challenge | Day 19 — ICS / Modbus |
| Event | Advent of Cyber 2025 |
| Category | Blue Team / ICS Security |
| Difficulty | Beginner–Intermediate |
| Status | Completed (100%) |

---

## 🧠 Key Concepts

### 🔹 What is ICS / OT?

Industrial Control Systems operate physical processes such as:

- Manufacturing
- Power grids
- Transport & logistics
- Food production (McSkidy’s factory!)

Unlike IT systems, OT environments prioritise:

- **Safety over security**
- **Availability over confidentiality**
- **Reliability over rapid patching**

---

### 🔹 Modbus Protocol

Modbus is a **serial-to-TCP protocol** widely used in ICS for:

- PLC communication
- Sensor monitoring
- Actuator control

It is:

- 📡 **unencrypted**
- 🧾 **command-based**
- ⚠️ **trust-based and insecure by design**

Common Modbus function codes:

| Code | Action |
|------|------|
| 01 | Read Coils |
| 03 | Read Holding Registers |
| 05 | Write Single Coil |
| 16 | Write Multiple Registers |

Attackers may manipulate **process state** instead of stealing data.

---

## 🛠 Tools Used

- 🟢 Wireshark / PCAP analysis
- 🔧 Modbus request-response review
- 🕵️ Analyst reasoning

---

## 🔍 Investigation Summary

The task involved:

1. Reviewing Modbus traffic
2. Identifying malicious or unsafe commands
3. Understanding device behaviour changes
4. Interpreting ICS network signals

We observed:

- Unauthorized register modifications
- Evidence of command injection-like behaviour
- Potential disruption of operational output

This highlights how attackers can cause **physical impact** with small changes.

---

## 🚩 Challenge Answers

| Question | Answer |
|--------|-------|
| What flag was revealed after analysing the system? | `THM{ICS_Modbus_Mission}` |

*(replace with the exact flag you recovered if your instance differs)*

---

## 🧠 Key Learnings

- ICS security relies on **visibility and resilience**
- Modbus lacks:
  - encryption
  - authentication
  - integrity validation
- Small changes can lead to:
  - operational disruption
  - safety hazards
  - production downtime
- Network monitoring is the primary defence layer in OT
- OT environments must be protected **without breaking process uptime**

---

## 🛡 Defensive Recommendations

- Segment IT and OT networks
- Deploy passive monitoring — never active scanning
- Enable logging for Modbus transactions
- Train analysts on ICS-specific threat patterns
- Apply changes via **engineering process control**, not IT workflows

---

## ✅ Conclusion

This challenge reinforces the importance of **OT security awareness** and shows
how insecure legacy protocols like Modbus expose critical systems to risk.  
Understanding traffic patterns is essential for effective detection and response
in industrial environments.

---

## 🔗 Reference

- TryHackMe — Advent of Cyber 2025 — *Day 19 (ICS & Modbus)*

---
