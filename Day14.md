# 🐳 TryHackMe Write-Up  
## Containers – DoorDasher’s Demise  
*(Advent of Cyber 2025 – Day 14)*

---

## 📌 Overview

This room introduces **container security concepts** and demonstrates how
misconfigured Docker environments can allow attackers to perform a
**container escape** and gain access to privileged containers.

The challenge walks through analysing running containers, interacting with
Docker from inside a container, abusing exposed **Docker runtime sockets**, and
finally restoring the compromised DoorDasher application.

---

## 🧩 Room Information

| Field | Details |
|------|--------|
| Platform | TryHackMe |
| Room Name | Containers – DoorDasher’s Demise |
| Event | Advent of Cyber 2025 |
| Category | Cloud / Container Security |
| Difficulty | Beginner–Intermediate |
| Estimated Time | ~60 minutes |
| Status | Completed (100%) |

---

## 🎯 Objectives

- Understand **containerisation vs virtual machines**
- Explore Docker container architecture & runtime sockets
- Enumerate running containers using Docker CLI
- Access an internal container shell
- Identify misconfigured Docker socket permissions
- Perform a **Docker escape technique**
- Access a privileged container and recover the service
- Restore the compromised web application

---

## 🧠 Container Security Concepts

### 🔹 What are Containers?

Containers package:

- An application  
- Its dependencies  
- Runtime environment  

…into an **isolated, lightweight execution environment**, sharing the host
kernel instead of running a full guest OS like virtual machines.

---

### 🔹 Containers vs Virtual Machines

| Containers | Virtual Machines |
|----------|----------------|
Lightweight, fast startup | Heavy, full OS per VM  
Share host kernel | Independent OS kernel  
Great for microservices | Best for legacy isolation  
Scale easily | Higher resource cost |

---

### 🔹 Docker Architecture

Docker operates as:

- **Client CLI** → sends commands  
- **Daemon** → manages containers  
- **Runtime socket (`/var/run/docker.sock`)** → API interface  

If a container can access the Docker socket, it may control the **entire host**
— enabling container escape.

---

## 🔍 Walkthrough

---

## 🧪 Step 1 — Enumerate Running Containers

docker ps
This revealed multiple running services including:

uptime-checker

deployer

Web app containers

The main web app at port 5001 was defaced as Hopperoo.

🧾 Step 2 — Access the uptime-checker Container

docker exec -it uptime-checker sh
Check Docker socket access:

ls -la /var/run/docker.sock
The container had direct access to the Docker runtime socket, meaning:

✔ The container can control Docker
✔ A container escape path exists

Running:

docker ps
confirmed full Docker control from inside the container.

🚀 Step 3 — Access the Privileged deployer Container

docker exec -it deployer bash
whoami
We now had access as a privileged user inside the deployment container.

A secret recovery script was located in /.

🛠 Step 4 — Restore the DoorDasher Application

sudo /recovery_script.sh
Refreshing the site confirmed the service was restored.
A flag was located in the same directory and retrieved using:

cat /flag.txt
🚩 Flags & Answers
Question	Answer
Command to list running Docker containers	docker ps
File defining Docker image build instructions	Dockerfile
Challenge flag	THM{DOCKER_ESCAPE_SUCCESS}
Bonus — deployer password from news site	DeployMaster2025!

🧠 Key Learnings
Containers share the host kernel — poor isolation = high risk

Docker socket exposure enables host-level compromise

Privileged containers must be protected

Enhanced container isolation should be enforced

Microservices adopted at scale require secure runtime design

🔐 Security Takeaway
A container with access to the Docker socket is effectively root on the
host — secure configuration and isolation are critical in production
environments.

✅ Conclusion
This room provided practical experience in container security, runtime abuse,
and recovery of a compromised application environment, highlighting the real-world
impact of Docker misconfigurations.

🔗 References
TryHackMe — Containers: DoorDasher’s Demise
