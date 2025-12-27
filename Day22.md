# 🎄 TryHackMe Advent of Cyber 2025  
## Day 22 — C2 Detection: Command & Carol

### 🕵️ Detecting Command-and-Control Traffic with RITA

This challenge focuses on detecting Command-and-Control (C2) communication by analysing network traffic using **RITA (Real Intelligence Threat Analytics)** and **Zeek logs**. The goal is to identify suspicious beaconing behaviour, malicious domains, and abnormal network communication patterns.

---

## 🧰 Tools & Concepts Used

- **RITA** — detects:
  - Beaconing behaviour
  - DNS tunnelling
  - Long-duration connections
  - Suspicious host communication
- **Zeek** — converts PCAP files into structured logs
- **Threat Intelligence Correlation**
- **Network Beacon Pattern Analysis**

RITA evaluates network events using **Threat Modifiers**, such as:

- 🧩 Rare TLS signatures  
- 🕒 Long connection duration  
- 🌍 Low-prevalence external hosts  
- 📡 Excessive subdomain contact  
- 🧾 Matches in threat-intel feeds  

---

## 🧪 Lab Workflow

### **Step 1 — Convert PCAP to Zeek Logs**

zeek readpcap pcaps/AsyncRAT.pcap zeek_logs/asyncrat
cd ~/zeek_logs/asyncrat && ls
Step 2 — Import Logs into RITA

rita import --logs ~/zeek_logs/asyncrat/ --database asyncrat
rita view asyncrat
RITA reports include:

Source & destination hosts

Beacon likelihood score

Connection duration

Subdomain communication

Threat-intel severity

Step 3 — Investigating Suspicious Behaviour
Two major findings stood out:

1️⃣ A long-duration FQDN communicating via Cloudflare Workers infra
2️⃣ A malicious external IP showing repeated periodic connections

Indicators observed:

Uncommon TLS signatures

Beacon-like timing patterns

Repeated domain contacts

Known-bad domain match

➡️ Strong evidence of C2 beaconing activity

🎯 Challenge PCAP Investigation
We repeated the same workflow on:

rita_challenge.pcap
and identified compromised hosts communicating with a malicious domain.

📝 Challenge Answers
Question	Answer
How many hosts communicate with malhare.net?	6
Threat Modifier showing number of hosts contacting destination	prevalence
Highest number of connections to rabbithole.malhare.net	40
Search filter for ≥70% beaconing sorted by duration	dst:rabbithole.malhare.net beacon:>=70 sort:duration-desc
Port used by 10.0.0.13 to connect	80

🛡️ Key Takeaways
C2 traffic is low-noise but persistent

Beacon timing patterns reveal compromised hosts

Zeek + RITA provide strong behaviour-based detection

Threat-intel feeds help validate suspicion

Long-running repetitive traffic = high-risk indicator

✅ Room Completed
This exercise reinforced practical network-forensics skills and demonstrated how analytical detection outperforms static signatures when uncovering covert adversary communications.

