# 🎣 TryHackMe Write-Up  
## Phishing — Phishmas Greetings  
*(Advent of Cyber 2025 – Day 12)*

---

## 📌 Overview

This room focuses on **phishing awareness and email triaging**, helping analysts
identify malicious messages sent by **Malhare’s Eggsploit Bunnies** to TBFC users.

The challenge reinforces how attackers leverage:

- Impersonation  
- Spoofing  
- Social engineering  
- Typosquatting & punycode  
- Malicious attachments  
- Fake login pages  
- Side-channel communication scams  

The objective is to review emails, classify messages as **spam vs phishing**, and
identify signals that reveal the attacker’s intent.

---

## 🧩 Room Information

| Field | Details |
|------|--------|
| Platform | TryHackMe |
| Room Name | Phishing — Phishmas Greetings |
| Event | Advent of Cyber 2025 |
| Category | Social Engineering / Phishing |
| Difficulty | Beginner |
| Estimated Time | ~30 minutes |
| Status | Completed (100%) |

---

## 🎯 Objectives

- Differentiate **spam vs phishing**
- Identify key phishing indicators
- Understand social engineering tactics
- Inspect sender domains & header metadata
- Recognise **punycode & typosquatting domains**
- Detect spoofed email identities
- Analyse **malicious links & attachments**

---

## 🧠 Key Phishing Concepts

### 🔹 Phishing vs Spam

| Spam | Phishing |
|------|---------|
Bulk unwanted marketing | Targeted deception
Annoying but mostly harmless | Credential theft, fraud, malware
No malicious intent | Clear malicious objective

---

### 🔹 Common Phishing Tactics

- **Impersonation** → pretending to be IT, HR, managers  
- **Urgency & pressure language**  
- **Credential harvesting links**  
- **Fake shared documents**  
- **Malware attachments (.html / .hta / .exe)**  
- **Spoofed email headers (SPF/DKIM/DMARC failures)**  
- **Side-channel messaging requests (WhatsApp, SMS, etc.)**

---

### 🔹 Typosquatting & Punycode

Attackers register **look-alike domains**, for example:

tbfc-it.com → legitimate
tbfc-ιt.com → punycode look-alike


Users may not visually notice differences — increasing success likelihood.

---

### 🔹 Spoofing Indicators

Check:

- `From:` vs `Return-Path`
- SPF / DKIM / DMARC status
- Mismatched sending domain
- Failed authentication results

If these fail → **high spoofing probability**

---

## 🔍 Investigation Walkthrough

During the activity, each email was reviewed and classified based on visible
signals such as:

- impersonation attempts  
- spoofed sender identity  
- malicious attachment presence  
- deceptive document sharing links  
- emotional manipulation & urgency wording  
- mismatched domains / punycode usage  

Correct classifications revealed a flag for each phishing email.

---

## 🚩 Flags Collected

| Email | Flag |
|------|------|
| Email #1 | `THM{yougotnumber1-keep-it-going}` |
| Email #2 | `THM{nmumber2-was-not-tha-thard!}` |
| Email #3 | `THM{Impersonation-is-areal-thing-keepIt}` |
| Email #4 | `THM{Get-back-SOC-mas!!}` |
| Email #5 | `THM{It-was-just-a-sp4m!!}` |
| Email #6 | `THM{number6-is-the-last-one!-DX!}` |

---

## 🧠 Key Learnings

- Not every unwanted email is phishing — **intent matters**
- Sender identity must be verified via **domain & headers**
- Spoofed emails often fail **SPF/DKIM/DMARC**
- Social engineering relies on **emotion manipulation**
- Punycode & typosquatting disguise fake domains
- HTML attachments are common **credential-harvesting tools**
- Fake document-sharing links redirect to malicious pages

---

## 🔐 Security Takeaway

> Phishing isn’t just about malicious links — it exploits **human trust and urgency**.  
> Always verify sender identity, domain authenticity, and email headers before acting.

---

## ✅ Conclusion

This room strengthened practical phishing-analysis skills by requiring analysts
to classify messages, identify deception indicators, and understand attacker
tactics used in real-world social-engineering campaigns.

---

## 🔗 References

- TryHackMe — Phishmas Greetings (Advent of Cyber 2025)

---
