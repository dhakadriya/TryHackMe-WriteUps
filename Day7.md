# 🌐 TryHackMe Write-Up  
## Network Discovery – Scan-ta Clause  
*(Advent of Cyber 2025 – Day 07)*

---

## 📌 Overview

This room focuses on **network discovery and service enumeration**, teaching how
attackers and defenders identify exposed services using **port scanning,
network utilities, and on-host inspection**.

The challenge walks through discovering hidden services, interacting with them,
retrieving key fragments, and finally uncovering a hidden flag inside a
database.

---

## 🧩 Room Information

| Field | Details |
|------|--------|
| Platform | TryHackMe |
| Room Name | Network Discovery – Scan-ta Clause |
| Event | Advent of Cyber 2025 |
| Category | Networking / Enumeration |
| Difficulty | Beginner |
| Estimated Time | ~30 minutes |
| Status | Completed (100%) |

---

## 🎯 Objectives

- Perform TCP and UDP port scanning with Nmap  
- Discover hidden services beyond default ports  
- Interact with unknown services using Netcat  
- Retrieve files via FTP  
- Query DNS TXT records  
- Enumerate on-host listening services  
- Access MySQL locally and extract stored data  

---

## 🛠 Tools Used

- **Nmap** – Port scanning and service discovery  
- **FTP Client** – Anonymous access to exposed file service  
- **Netcat (nc)** – Manual interaction with custom TCP services  
- **dig** – DNS query tool  
- **ss / netstat** – On-host listening port discovery  
- **MySQL client** – Local database enumeration  

---

## 🔍 Walkthrough

---

## 🧪 Step 1: Basic Nmap Scan

A standard 1000-port TCP scan revealed:

nmap MACHINE_IP
livecodeserver
22/tcp  open  ssh
80/tcp  open  http
Visiting the website showed a defacement message:

Pwned by HopSec

🚀 Step 2: Full TCP Port Scan + Banner Detection
Scanning all ports identified additional services:
nmap -p- --script=banner MACHINE_IP
Results included:
FTP on port 21212
Custom TBFC service on port 25251

🗂 Step 3: Accessing FTP (Anonymous Login)
ftp MACHINE_IP 21212
Listing files revealed the first key fragment:
tbfc_qa_key1
→ 3aster_

🧾 Step 4: Interacting With Custom Service (Netcat)
nc -v MACHINE_IP 25251
After issuing the correct command, the second key fragment was recovered:
→ 15_th3_

🛰 Step 5: UDP Scan & DNS Enumeration
UDP services were scanned:
nmap -sU MACHINE_IP
Port 53/udp was open (DNS). The TXT record was queried:
dig @MACHINE_IP TXT key3.tbfc.local +short
Result:
→ n3w_xm45
This provided the third key fragment.

🔑 Combined Access Key
3aster_15_th3_n3w_xm45
This was used to unlock the secret admin console on the web app.

🖥 Step 6: On-Host Service Discovery
Inside the console:
ss -tunlp
Detected a local MySQL service listening on:
3306

🗄 Step 7: MySQL Enumeration
mysql -D tbfcqa01 -e "select * from flags;"
The final flag was retrieved:
THM{4ll_s3rvice5_d1sc0vered}

🚩 Answers Collected
Question	Answer
Website defacement message	Pwned by HopSec
Key Part 1 (FTP)	3aster_
Key Part 2 (TBFC App)	15_th3_
Key Part 3 (DNS TXT Record)	n3w_xm45
MySQL Port	3306
Final Flag	THM{4ll_s3rvice5_d1sc0vered}

🧠 Key Learnings
Hidden services often run on non-standard ports

Full-range scanning is essential during investigations

UDP services can reveal important infrastructure data

Netcat is useful for interacting with unknown protocols

On-host enumeration provides visibility beyond remote scanning

Local-only services (like databases) can still expose critical data

🔐 Security Takeaway
Service discovery is a critical phase in both offensive security and
defensive monitoring. Exposed services — even on unusual ports — can
reveal sensitive systems and hidden attack paths.

✅ Conclusion
This room provided practical experience in network enumeration, service
discovery, and post-access inspection, reinforcing the importance of thorough
scanning techniques in real-world cyber operations.

🔗 References
TryHackMe Room – Network Discovery: Scan-ta Clause
https://tryhackme.com/room/networkdiscovery
