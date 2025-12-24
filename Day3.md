# 📊 TryHackMe Write-Up  
## Splunk Basics – Did you SIEM?  
*(Advent of Cyber 2025 – Day 03)*

---

## 📌 Overview

This room is part of **TryHackMe Advent of Cyber 2025** and focuses on
**log analysis and incident investigation using Splunk**.  
The objective is to analyze pre-ingested web and firewall logs to identify
a real-world cyber attack, trace the attacker’s actions, and confirm
post-exploitation activity such as command-and-control (C2) communication.

---

## 🧩 Room Information

| Field | Details |
|-----|---------|
| Platform | TryHackMe |
| Room Name | Splunk Basics – Did you SIEM? |
| Event | Advent of Cyber 2025 |
| Category | SIEM / Log Analysis |
| Difficulty | Beginner – Intermediate |
| Estimated Time | ~60 minutes |
| Status | Completed (100%) |

---

## 🎯 Objectives

- Explore and understand Splunk’s Search & Reporting interface  
- Analyze web and firewall logs  
- Detect anomalies and suspicious behavior  
- Trace the full attack lifecycle  
- Quantify data exfiltration  
- Identify the attacker and attack timeline  

---

## 🛠 Tools & Technologies

- **Splunk (Search & Reporting App)**
- **Splunk Processing Language (SPL)**
- **Web Traffic Logs**
- **Firewall Logs**
- **Time-based & statistical analysis**

---

## 📂 Data Sources Investigated

Two datasets were pre-ingested into Splunk:

| Sourcetype | Description |
|-----------|-------------|
| `web_traffic` | Web server access logs |
| `firewall_logs` | Allowed/blocked network traffic |

The internal web server IP was identified as **10.10.1.5**.

---

## 🔍 Investigation Walkthrough

---

## 🧪 Initial Triage

To view all ingested logs:

```spl
index=main
To focus on web traffic:

spl
index=main sourcetype=web_traffic
This revealed a traffic spike, indicating a potential attack window.

📈 Timeline Analysis
To visualize daily event volume:

spl
index=main sourcetype=web_traffic | timechart span=1d count
Sorting results identified the peak traffic day:

spl
index=main sourcetype=web_traffic | timechart span=1d count | sort by count | reverse
📅 Peak attack date: 2025-10-12

🚨 Anomaly Detection
Suspicious User Agents
Legitimate browser traffic was filtered out:

spl
index=main sourcetype=web_traffic user_agent!=*Mozilla* user_agent!=*Chrome* user_agent!=*Safari* user_agent!=*Firefox*
This exposed automated tools and scanners.

Attacker IP Identification
To find the most active suspicious IP:

spl
sourcetype=web_traffic user_agent!=*Mozilla* user_agent!=*Chrome* user_agent!=*Safari* user_agent!=*Firefox*
| stats count by client_ip
| sort -count
| head 5
📌 Attacker IP identified: 198.51.100.55

🔗 Attack Chain Analysis
1️⃣ Reconnaissance
Probing for exposed files:

spl
sourcetype=web_traffic client_ip="198.51.100.55"
AND path IN ("/.env","/*phpinfo*","/.git*")
| table _time path user_agent status
2️⃣ Enumeration & Vulnerability Testing
Path traversal and redirect attempts:

spl
sourcetype=web_traffic client_ip="198.51.100.55"
AND path="*..\/..\/*" OR path="*redirect*"
| stats count by path
📊 Path traversal attempts: 658

3️⃣ SQL Injection
Detection of automated SQL injection tools:

spl
sourcetype=web_traffic client_ip="198.51.100.55"
AND user_agent IN ("*sqlmap*","*Havij*")
| table _time path status
📊 Havij user-agent events: 993

A 504 status code confirmed time-based SQL injection attempts.

4️⃣ Data Exfiltration
Attempts to download large archives:

spl
sourcetype=web_traffic client_ip="198.51.100.55"
AND path IN ("*backup.zip*","*logs.tar.gz*")
| table _time path user_agent
5️⃣ Ransomware Staging & RCE
Evidence of webshell execution and payload delivery:

spl
sourcetype=web_traffic client_ip="198.51.100.55"
AND path IN ("*bunnylock.bin*","*shell.php?cmd=*")
| table _time path user_agent status
This confirmed Remote Code Execution (RCE) and ransomware staging.

🔄 C2 Communication Correlation
Pivoting to firewall logs:

spl
sourcetype=firewall_logs
src_ip="10.10.1.5"
AND dest_ip="198.51.100.55"
AND action="ALLOWED"
| table _time action protocol src_ip dest_ip dest_port reason
✔ Confirmed outbound C2 communication.

📦 Data Exfiltration Volume
To calculate total data transferred:

spl
sourcetype=firewall_logs
src_ip="10.10.1.5"
AND dest_ip="198.51.100.55"
AND action="ALLOWED"
| stats sum(bytes_transferred) by src_ip
📊 Bytes exfiltrated: 126167

🚩 Final Answers
Question	Answer
Attacker IP	198.51.100.55
Peak attack date	2025-10-12
Havij events	993
Path traversal attempts	658
Bytes exfiltrated	126167

🧠 Key Learnings
SIEM tools are critical for incident detection and response

Correlating multiple log sources reveals the full attack lifecycle

SPL enables powerful filtering, aggregation, and visualization

Attackers follow predictable phases: recon → exploit → exfiltration → C2

🔐 Security Takeaway
Effective monitoring and log correlation can uncover even sophisticated
attacks. SIEM solutions like Splunk play a vital role in modern blue-team
operations.

✅ Conclusion
This room provided hands-on experience in real-world incident analysis using Splunk.
By tracing the attacker from reconnaissance to ransomware staging and C2
communication, it demonstrated the importance of log visibility, correlation,
and analytical thinking in cybersecurity defense.

🔗 References
TryHackMe Room:
https://tryhackme.com/room/splunkbasics
