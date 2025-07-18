📁 OWASP-Juice-Shop-Confidential-Document-Challenge/Method-03_DevTools-Network-Inspection/README.md
markdown
Copy code
# 🕵️‍♂️ Method 03 - DevTools Network Inspection

## 🎯 Objective
Manually inspect network requests using browser DevTools to uncover hidden or unauthorized files (e.g., `.md`, `.zip`, `.conf`, `.bak`, etc.) that may be exposed on the Juice Shop server.

---

## 🧰 Tools Required
- Google Chrome or Firefox
- OWASP Juice Shop running locally
- Basic knowledge of DevTools > Network tab

---

## 🪜 Step-by-Step Guide

### 1. Launch Juice Shop
```bash
docker run -d -p 3000:3000 --name juice-shop bkimminich/juice-shop
Open browser and visit:
http://localhost:3000

2. Open DevTools
Press F12 or right-click > "Inspect"

Go to the Network tab

Refresh the page (Ctrl + R) to load all requests

3. Monitor Requests
Watch for:

Suspicious file names

*.md, *.bak, *.zip, etc.

Unauthenticated GET requests with successful (200 OK) responses

4. Look Into JS Files
Inspect loaded JS files under the Sources tab

Look for:

Hidden API paths

Hardcoded links

Clues to sensitive endpoints

5. Visit Discovered URLs
Manually visit any unusual or sensitive paths seen in requests

Example:

http
Copy code
http://localhost:3000/ftp/acquisitions.md
Use curl or browser to verify and download if needed

🧪 Sample Outcome
You might discover:

/ftp/acquisitions.md

/support/logs/backup.zip

/confidential/internal.txt

🔐 Mitigation Advice
Remove public access to sensitive files

Obfuscate internal APIs

Apply role-based authentication

📁 Folder Contents
README.md: This file

TODO.md: Step checklist

screenshots/: Optional folder for captured DevTools screenshots

findings.md: Summary of discovered paths and risks
