📁 OWASP-Juice-Shop-Confidential-Document-Challenge/Method-07_Common-File-Name-Heuristics/README.md
markdown
Copy code
# 🧠 Method 07: Common File Name Heuristics

## 🎯 Goal
Exploit predictable naming conventions used by developers to uncover hidden or sensitive files (e.g., backups, temporary files, internal docs) that may lead to the disclosure of confidential information.

## 🧩 Strategy Overview

Most developers unknowingly leave behind files with common patterns such as:
- `backup.zip`, `config.old`, `secret.txt`, `admin.bak`
- File extensions like `.old`, `.bak`, `.swp`, `.tmp`, `.tar.gz`, etc.

The objective is to apply offensive enumeration heuristics to discover such artifacts using smart wordlists and HTTP fuzzing.

---

## 🛠️ Step-by-Step Technical Walkthrough

### 🔍 Step 1: Define Scope
Confirm base URL of Juice Shop (usually `http://<TARGET_IP>:3000/`).

```bash
export TARGET=http://<TARGET_IP>:3000
📂 Step 2: Use FFUF to Enumerate Predictable Filenames
Use a curated wordlist that contains filenames likely to expose secrets. Example:

bash
Copy code
ffuf -w /usr/share/wordlists/raft-large-files.txt -u $TARGET/FUZZ -mc 200,206,403,401 -t 50 -v
🔍 Explanation:

-w: Wordlist of common filenames

-u: URL to fuzz

-mc: Match HTTP status codes (200 OK, 206 Partial Content, 403 Forbidden, 401 Unauthorized)

-t: Threads for speed

-v: Verbose output for better traceability

💡 Tip: Add extensions using -e flag:

bash
Copy code
-e .bak,.old,.zip,.tar.gz,.txt,.conf,.md,.pdf
🛠️ Step 3: Analyze Results
Look out for:

200 OK responses: File accessible directly

403 Forbidden: Might still be accessible via header manipulation

Files containing keywords like confidential, internal, secret, etc.

Example:

bash
Copy code
http://<TARGET>:3000/confidential_backup.zip
http://<TARGET>:3000/internal_readme.old
Use tools like file, strings, or binwalk to extract useful content if files are binary or compressed.

🧬 Backend Technical Insight
Most Node.js/Express applications serve static files via express.static() or middleware. If no access control middleware (e.g., authGuard) is applied to non-route directories, they are publicly exposed. These are often missed in manual secure code reviews.

🛡️ Why This Works (Vulnerability Principle)
Security Misconfiguration (OWASP Top 10 - A05)

Insufficient File Access Controls

Lack of Secure DevOps Practices

Reliance on security by obscurity — i.e., "no one will guess that filename"

✅ Outcome
You should be able to:

Identify and retrieve confidential documents directly from exposed file paths

Map risky naming patterns used by developers

Use it to pivot toward privilege escalation or lateral discovery (e.g., leaked credentials)

🧠 Bonus Tip
If your enumeration is noisy or hitting rate limits, add headers to blend in:

bash
Copy code
-H "User-Agent: Mozilla/5.0" -H "Referer: $TARGET"
Use Burp Suite, ZAP, or GoBuster as alternatives if needed.

kotlin
Copy code
