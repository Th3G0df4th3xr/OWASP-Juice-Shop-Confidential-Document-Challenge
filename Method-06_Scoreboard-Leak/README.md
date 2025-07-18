📁 OWASP-Juice-Shop-Confidential-Document-Challenge/Method-06_Scoreboard-Leak/README.md
markdown
Copy code
# 🧠 Method 06 - Scoreboard Leak Exploitation

## 🎯 Objective:
Leverage the exposure of the internal **Scoreboard** page in OWASP Juice Shop to identify confidential challenges or endpoints inadvertently revealed via the vulnerable interface.

---

## ⚙️ Technical Background:

The **Scoreboard** is a hidden administrative feature in Juice Shop used to track solved challenges. It is *not meant to be publicly accessible* in real-world scenarios, as it often references sensitive challenge logic, hint endpoints, or undocumented functionalities. When exposed, it acts as a low-hanging fruit for **Information Disclosure** vulnerabilities and aids adversaries in footprinting and strategy formulation.

---

## 🧪 Step-by-Step Methodology:

### 🧰 Step 1: Start OWASP Juice Shop in Terminal
```bash
docker run -d -p 3000:3000 --name juice-shop bkimminich/juice-shop
✅ This starts the Juice Shop on http://localhost:3000

🔍 Monitor with docker logs -f juice-shop for runtime output

🔎 Step 2: Discover the Scoreboard Path
Option 1: Bruteforce Method (using ffuf, gobuster, etc.)
bash
Copy code
ffuf -u http://localhost:3000/FUZZ -w /usr/share/wordlists/dirb/common.txt -mc 200
Look for score-board or similar paths.

-mc 200 filters only for successful responses (HTTP 200)

Option 2: Manual JavaScript Inspection
Open Developer Tools (F12) > Sources > main.js

Search for /score-board in frontend routing definitions

You may find lines like:

js
Copy code
$routeProvider.when('/score-board', ...)
This confirms the hidden route exists.

🔐 Step 3: Access the Scoreboard
bash
Copy code
curl http://localhost:3000/score-board
or open in browser:

bash
Copy code
http://localhost:3000/score-board
📌 Expected: A page listing all solved/unsolved challenges

🛑 Problem: This page should be protected or hidden from users in production

🧠 Step 4: Analyze Challenge Names and Clues
Look for challenge titles like:

Confidential Document

Forgotten Dev Backup

Log Injection, etc.

These names can hint toward backend functionality or paths like:

/ftp/

/support/logs.zip

/confidential.pdf

💡 Strategy: Use this intelligence to pivot and try new paths or test for vulnerabilities mentioned.

📸 Step 5: Screenshot & Documentation
Use tools like gnome-screenshot or browser snip tools

Note the date, access path, and challenge names revealed

🛡️ Remediation (Defensive Context)
Protect internal routes like /score-board via proper RBAC

Ensure hidden pages are not discoverable via routing or source code exposure

Disable scoreboard in production builds or gate it behind authentication

📚 Offensive Concepts Covered
Concept	Explanation
Information Disclosure	Revealing internal mechanisms or endpoints unintended for public access
Insecure Configuration	Application exposes developer-only tools or debug panels
Client-Side Routing Discovery	Identifying paths hardcoded in JS routing logic
Lateral Discovery	Using one misconfiguration to explore other vulnerabilities

✅ Success Criteria
 Able to enumerate /score-board

 Derived at least one hidden path or endpoint

 Understood why this route aids in challenge-solving

 Suggested mitigation for scoreboard exposure

