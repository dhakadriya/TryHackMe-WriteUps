# 🛰️ TryHackMe Write-Up  
## Web Attack Forensics — Drone Alone  
*(Advent of Cyber 2025 — Day 15)*

---

## 📌 Overview

This room focuses on **web attack forensics using Splunk**, analysing web-server,
error, and Sysmon logs to trace a **command-injection attack** against a CGI
script. The investigation follows how the attacker executed commands through
Apache, confirmed execution via process logs, and attempted to run encoded
PowerShell payloads.

The challenge reinforces blue-team workflows for:

- Web log investigation  
- Command-injection detection  
- Process lineage correlation  
- Post-exploitation activity analysis  
- PowerShell encoded-command detection  

---

## 🧩 Room Information

| Field | Details |
|------|--------|
| Platform | TryHackMe |
| Room Name | Web Attack Forensics — Drone Alone |
| Event | Advent of Cyber 2025 |
| Category | Blue Team / Log Analysis |
| Difficulty | Beginner–Intermediate |
| Estimated Time | ~30 minutes |
| Status | Completed (100%) |

---

## 🎯 Objectives

- Investigate web attack behaviour in Splunk  
- Identify **command-injection attempts** via web logs  
- Trace backend execution via **Sysmon process logs**  
- Confirm attacker reconnaissance actions  
- Detect possible **Base64-encoded PowerShell payloads**  

---

## 🛠️ Environment

- Tool: **Splunk**
- Logs reviewed:
  - `windows_apache_access`
  - `windows_apache_error`
  - `windows_sysmon`

Time range was expanded to ensure events were visible.

---

## 🔍 Investigation Walkthrough

---

### 🧪 Step 1 — Detect Suspicious Web Requests

Query:

index=windows_apache_access (cmd.exe OR powershell OR "powershell.exe" OR "Invoke-Expression")
| table _time host clientip uri_path uri_query status
Findings:

Requests targeted /cgi-bin/hello.bat

Parameters attempted to execute cmd.exe / PowerShell

Included Base64-encoded payloads

Decoded payload revealed:

This is now Mine! MUAHAHAHA
Indicating attacker-placed output.

🧾 Step 2 — Check Apache Error Logs
Query:

index=windows_apache_error ("cmd.exe" OR "powershell" OR "Internal Server Error")
Findings:

HTTP 500 errors occurred when commands were processed

Confirms input reached backend execution layer

⚙️ Step 3 — Trace Process Creation from Apache
Query:

index=windows_sysmon ParentImage="*httpd.exe"
Findings:

Apache spawned unexpected processes:

Parent: C:\Apache24\bin\httpd.exe
Child : C:\Windows\System32\cmd.exe
This is strong evidence of successful command injection.

🕵️ Step 4 — Confirm Enumeration Activity
Query:

index=windows_sysmon *cmd.exe* *whoami*
Findings:

Attacker executed:

whoami.exe
Indicating post-exploitation reconnaissance.

➡️ Answer: whoami.exe

🔐 Step 5 — Search for Encoded PowerShell Commands
Query:

index=windows_sysmon Image="*powershell.exe" (CommandLine="*enc*" OR CommandLine="*-EncodedCommand*" OR CommandLine="*Base64*")
Expected result: No matches (payload blocked).

If found → decode to reveal intent.

➡️ Executable attempted via injection:
PowerShell.exe

🚩 Answers Collected
Question	Answer
Reconnaissance executable name	whoami.exe
Executable attempted via injection	PowerShell.exe

🧠 Key Learnings
Command injection leaves traces across:

Web access logs

Error logs

Sysmon process logs

Parent/child process relationships prove execution

Attackers commonly:

Run whoami

Use Base64-encoded PowerShell payloads

Lack of results in encoded-payload searches = blocked execution

🔐 Defensive Takeaways
Restrict CGI execution

Validate & sanitize URL parameters

Enable Sysmon process lineage logging

Monitor:

cmd.exe and powershell.exe spawned by web services

Base64 or -EncodedCommand flags

Deploy WAF protections where possible

✅ Conclusion
This room provided hands-on experience in forensic investigation of web-based
command injection, showing how Blue Teams correlate logs to reconstruct attack
paths and confirm execution activity.

🔗 References
TryHackMe — Drone Alone (Web Attack Forensics)
