📁 OWASP-Juice-Shop-Confidential-Document-Challenge/Method-02_FFUF-Dirb-Dirsearch/README.md
markdown
Copy code
# 🚀 Method 02: Directory Brute Forcing Using FFUF / Dirb / Dirsearch

## 🎯 Objective
Use directory enumeration tools like **FFUF**, **Dirb**, or **Dirsearch** to discover hidden directories or confidential documents in the OWASP Juice Shop instance — simulating a real-world attacker’s enumeration phase.

---

## 🧪 Tools Used
- 🔍 `ffuf` – Fast web fuzzer written in Go.
- 🕵️ `dirb` – Simple web content scanner.
- 🧭 `dirsearch` – Python-based directory scanner.

---

## 🛠️ Environment Setup
Ensure Juice Shop is running locally:

```bash
docker run -d -p 3000:3000 --name juice-shop bkimminich/juice-shop
Default URL: http://localhost:3000

🚨 Attack Strategy
🔹 FFUF Command Example:
bash
Copy code
ffuf -u http://localhost:3000/FUZZ -w /usr/share/wordlists/dirb/common.txt -t 50 -o ffuf_results.json
🔹 Dirb Command Example:
bash
Copy code
dirb http://localhost:3000 /usr/share/wordlists/dirb/common.txt
🔹 Dirsearch Command Example:
bash
Copy code
python3 dirsearch.py -u http://localhost:3000 -e txt,md,js,json,zip,backup -x 404
🔍 What to Look For:
/ftp/

/backup/

Any .md, .bak, .zip, .old or .confidential documents

Check for:

acquisitions.md

legal.md

Directory traversal potential

📈 Outcome
If the brute force discovers:

Hidden paths like /ftp/

Confidential markdown documents like acquisitions.md

...you've cracked this method.

🛡️ Security Insight
Directory brute-forcing is often effective against misconfigured servers. Lack of:

Access Controls

Proper Index Restrictions

File permission checks

...leads to Information Disclosure & IDOR vulnerabilities.

✅ Validation
HTTP 200 OK for sensitive paths

File content revealing sensitive business logic or internal operations

🧩 Recommended Fix
Restrict direct access to static sensitive files

Implement proper access control and authorization

Disable directory listing on web servers

Monitor and rate-limit enumeration attempts

