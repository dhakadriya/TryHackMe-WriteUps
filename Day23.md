# 🎄 TryHackMe Advent of Cyber 2025  
## Day 23 — AWS Security: S3cret Santa

### ☁️ AWS Enumeration & S3 Bucket Access Walkthrough

This challenge introduces AWS Identity & Access Management (IAM) concepts and demonstrates how misconfigured permissions can allow attackers to enumerate users, assume roles, and exfiltrate files from an S3 bucket.

The exercise focuses on discovering accessible AWS resources, escalating access using role assumption, and retrieving sensitive data stored inside an S3 bucket.

---

## 🧰 Key AWS Concepts

- **IAM Users** — Individual identities with credentials
- **IAM Roles** — Temporary identities that accounts can assume
- **Policies** — Define what an identity can or cannot do
- **S3 (Simple Storage Service)** — Cloud object storage using “Buckets”

S3 buckets often store:

- Website assets
- Logs & backups
- Application files
- Internal documentation

If permissions are misconfigured… attackers may gain access.

---

## 🧪 Practical Walkthrough

### ✅ Step 1 — List Available S3 Buckets

aws s3api list-buckets
A bucket referencing easter was identified as interesting.

✅ Step 2 — List Objects Inside the Bucket

aws s3api list-objects --bucket easter-secrets-123145
A suspicious file was discovered:

cloud_password.txt
✅ Step 3 — Download the File

aws s3api get-object \
--bucket easter-secrets-123145 \
--key cloud_password.txt \
cloud_password.txt
The file contained sensitive credentials — confirming successful
enumeration and data exfiltration.

🚩 Challenge Answer
Question	Answer
What IAM component is used to describe the permissions to be assigned to a user or a group?    policy
What is the name of the policy assigned to sir.carrotbane?   SirCarrotbanePolicy
Apart from GetObject and ListBucket, what other action can be taken by assuming the bucketmaster role?   ListAllMyBuckets
Contents of cloud_password.txt	THM{more_like_sir_cloudbane}

🛡️ Security Lessons
Misconfigured IAM policies expose sensitive data

Even low-privileged users may list or access buckets

S3 bucket permissions must follow least-privilege

Monitor:

ListObjects & GetObject events

Suspicious role assumptions

Cross-account access attempts

Defensive best practices:

Disable public bucket access unless required

Use IAM Access Analyzer

Enable CloudTrail logging

Enforce MFA & strong role separation

✅ Room Completed
This challenge reinforces real-world AWS security enumeration and highlights the risks of weak privilege boundaries across IAM and S3 services.
