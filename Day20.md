# 🏎️ TryHackMe Write-Up  
## Race Conditions — Toy to The World  
*(Advent of Cyber 2025 — Day 20)*

---

## 📌 Overview

This challenge introduces **race condition vulnerabilities** in web applications
and demonstrates how attackers can exploit poorly synchronized operations to
bypass logical controls — in this case, overselling a limited-edition SleighToy.

Using **Burp Suite Repeater**, we reproduce a real-world concurrency exploit by
sending multiple parallel checkout requests to trigger inconsistent inventory
updates.

---

## 🧩 Room Information

| Field | Details |
|------|--------|
| Platform | TryHackMe |
| Room | Race Conditions — Toy to The World |
| Event | Advent of Cyber 2025 |
| Category | Web Security / Logic Bugs |
| Difficulty | Beginner–Intermediate |
| Time | ~30 minutes |
| Status | Completed (100%) |

---

## 🧠 Key Concepts

### 🔹 What is a Race Condition?

A race condition occurs when two or more actions execute at the same time and the
final result depends on **which one finishes first**.

In web applications, this happens when multiple requests modify the same shared
resource simultaneously — such as:

- Inventory stock
- Account balance
- Order processing
- Rewards / coupon usage

If the application fails to synchronize operations, it may:

- Process duplicate purchases
- Oversell items
- Commit transactions inconsistently
- Update records out of order

---

### 🔹 Types of Race Conditions Covered

- **TOCTOU (Time-of-Check-to-Time-of-Use)**  
  Validation and execution occur at different times.

- **Shared Resource Conflict**  
  Multiple requests modify the same value in parallel.

- **Atomicity Violation**  
  The transaction is not executed as a single indivisible operation.

---

## 🛠 Tools Used

- 🐙 Burp Suite Repeater  
- 🦊 Firefox + FoxyProxy  
- 💳 TBFC Web Shop Application

---

## 🔍 Exploitation Walkthrough

### 🟡 Step 1 — Perform a Normal Purchase

1. Login to the app using:
attacker / attacker@123

2. Add **SleighToy Limited Edition** to cart  
3. Checkout → Confirm & Pay  
4. Observe valid purchase behaviour

---

### 🟡 Step 2 — Capture Checkout Request

1. In **Burp Proxy → HTTP History**
2. Locate request:

POST /process_checkout


3. Right-click → **Send to Repeater**

---

### 🟡 Step 3 — Create Parallel Requests

1. In Repeater → **Add tab to group**
2. Name group: `cart`
3. Duplicate tab (≈15 copies)
4. Run:

Send group (parallel) — last-byte sync


✔ All checkout requests execute simultaneously  
✔ Stock validation fails  
✔ Orders process multiple times

---

### 🟡 Step 4 — View Result

- SleighToy stock drops below zero
- Multiple orders succeed
- Race condition successfully exploited 🎯

---

## 🚩 Flags Obtained

| Target Item | Flag |
|-----------|------|
| SleighToy Limited Edition | `THM{WINNER_OF_R@CE007}` |
| Bunny Plush (Blue) | `THM{WINNER_OF_Bunny_R@ce}` |

---

## 🧠 Key Learnings

- Race conditions are **logic flaws**, not injection exploits
- Validation alone is insufficient without **atomic processing**
- Parallel execution testing is essential during security assessments
- Burp Repeater is highly effective for concurrency testing

---

## 🛡 Mitigation Recommendations

- Use **atomic database transactions**
- Re-validate stock at commit time
- Implement **idempotency tokens** for checkout requests
- Apply **rate-limiting** & concurrency controls
- Queue sensitive operations instead of processing in parallel

---

## ✅ Conclusion

This exercise demonstrates how real-world business logic flaws can lead to
financial and operational impact — even without traditional vulnerabilities like
SQLi or XSS. Race-condition testing is a critical part of modern web security
assessments.

---

## 🔗 Reference

- TryHackMe — *Race Conditions: Toy to The World (AoC 2025)*

---
