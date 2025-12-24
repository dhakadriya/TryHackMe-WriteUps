# 🐧 TryHackMe Write-Up  
## Linux CLI – Shells & Bells  
*(Advent of Cyber 2025 – Day 1)*

---

## 📌 Overview

This room is part of **TryHackMe Advent of Cyber 2025** and focuses on building
foundational skills in the **Linux Command Line Interface (CLI)**.  
The objective is to investigate a compromised system, uncover hidden files,
analyze logs, inspect suspicious scripts, and trace attacker activity using
basic but powerful Linux commands.

---

## 🧩 Room Information

| Field | Details |
|------|--------|
| Platform | TryHackMe |
| Room Name | Linux CLI – Shells & Bells |
| Event | Advent of Cyber 2025 |
| Category | Linux Fundamentals / Blue Team |
| Estimated Time | ~30 minutes |
| Difficulty | Beginner |
| Status | Completed (100%) |

---

## 🎯 Objectives

- Navigate the Linux filesystem
- Identify hidden files
- Analyze system logs
- Investigate suspicious shell scripts
- Switch users and inspect bash history
- Retrieve flags left by the attacker

---

## 🛠 Tools & Commands Used

- `echo`
- `ls`, `ls -la`
- `pwd`, `cd`
- `cat`
- `grep`
- `find`
- `sudo su`
- `whoami`
- `history`

---

## 🔍 Investigation Walkthrough

---

## 🧪 Task 1: Getting Started with Linux CLI

To verify command execution and explore the environment:

```bash
echo "Hello World!"
ls
cat README.txt

## 📂 Task 2: Navigating the Filesystem

Moved into the Guides directory:

cd Guides
ls


The directory appeared empty, suggesting hidden files.

To reveal hidden content:

ls -la
cat .guide.txt


📄 The hidden guide warned about an attack and suggested checking logs and
looking for “eggs” (malicious artifacts).

## 📜 Task 3: Log Analysis

Navigated to system logs:

cd /var/log


Filtered authentication failures using grep:

grep "Failed password" auth.log


🔎 Observed repeated failed login attempts on the socmas account from a
suspicious external source.

## 🕵️ Task 4: Finding Suspicious Files

Searched for suspicious files related to “egg”:

find /home/socmas -name "*egg*"


Found a suspicious script:

/home/socmas/2025/eggstrike.sh

## 🧨 Task 5: Analyzing the Malicious Script

Displayed the script contents:

cat eggstrike.sh

Script Behavior Analysis:

Extracts unique wishlist entries

Dumps data to /tmp/dump.txt

Deletes the original wishlist

Replaces it with a malicious one (eastmas.txt)

This confirmed data manipulation and sabotage by the attacker.

## 👑 Task 6: Privilege Escalation & Root Access

Attempting to read sensitive files as a normal user resulted in denial:

cat /etc/shadow


Switched to root user:

sudo su
whoami


✔ Successfully gained root access.

## 📜 Task 7: Bash History Analysis

Checked root’s bash history for attacker traces:

cd /root
cat .bash_history


Found malicious curl commands uploading stolen data and reporting back to
attacker infrastructure.

## 🚩 Flags Collected
Listing directories	THM{learning-linux-cli}
Filtering logs	THM{sir-carrotbane-attacks}
Final attacker trace	THM{until-we-meet-again}

🧠 Key Learnings

Linux CLI is extremely powerful for investigations
Hidden files often contain critical information
Logs are essential for detecting intrusions
Shell scripts can be abused for malicious automation
Bash history can reveal attacker activity
Root privileges must be protected carefully

🔐 Security Takeaway

Many real-world breaches can be detected early by reviewing logs, user
activity, and hidden files. Mastering Linux CLI is essential for any
cybersecurity professional.

✅ Conclusion

This room provided hands-on exposure to Linux command-line investigation,
highlighting how attackers leave traces and how defenders can uncover them
using basic yet effective CLI tools.

🔗 References

TryHackMe Room:
https://tryhackme.com/room/linuxcli-shellsbells
