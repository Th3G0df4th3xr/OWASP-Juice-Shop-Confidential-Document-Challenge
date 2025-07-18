📄 OWASP-Juice-Shop-Confidential-Document-Challenge/Method-04_Source-Code-Review/README.md
markdown
Copy code
# 🛠️ Method 04: Source Code Review — Exploiting Misconfigurations & Insecure Disclosure via Static Analysis

## 🎯 Objective:
To locate hidden or sensitive endpoints by statically analyzing the OWASP Juice Shop source code. This method leverages the offensive security principle of **information disclosure via accessible source** and identifies exploitable artifacts or hardcoded paths that may reveal confidential files.

---

## ⚙️ Step-by-Step Technical Execution

### 🧩 Step 1: Clone the OWASP Juice Shop Repository

This gives access to the actual server-side and client-side code for full static analysis.

```bash
git clone https://github.com/juice-shop/juice-shop.git
cd juice-shop
🔍 Why: Gaining access to source code enables direct visibility into route handlers, API logic, and file path mappings. This is critical for spotting endpoints that aren't discoverable through scanning or browsing.

🔍 Step 2: Grep for Static File Paths and Route Handlers
You're specifically looking for express routes (app.get, app.use, etc.), and static file references like .md, .pdf, .bak.

bash
Copy code
grep -Ri "\.md\|\.pdf\|\.bak\|\.conf" .
💡 What This Does:
This command recursively searches for mentions of Markdown, PDF, backup, and config files. If a file like acquisitions.md or easter_egg.pdf is hardcoded in a route or static path, it may indicate a direct access vector.

Expected output:

css
Copy code
./routes/ftp.js: res.sendFile(path.resolve(__dirname, '../ftp/acquisitions.md'))
🛠 Step 3: Identify express.static or sendFile Usage
Look for:

js
Copy code
app.use('/ftp', express.static(path.join(__dirname, 'ftp')))
📌 Explanation:
The express.static() middleware exposes a directory directly via HTTP. If this middleware is configured to serve sensitive folders (like ftp/, support/, logs/), then all files within it become web-accessible.

Also check for res.sendFile() usage which sends specific files over HTTP directly.

bash
Copy code
grep -Ri "express.static" .
grep -Ri "sendFile" .
🔍 Step 4: Search for robots.txt, security.txt, or Debug Endpoints
bash
Copy code
grep -Ri "robots.txt" .
grep -Ri "security.txt" .
These files often indicate disallowed paths that are ironically useful to attackers for enumeration.

🔐 Step 5: Analyze JavaScript Files for Hidden API Calls
Most API calls originate from frontend/src/app/ and routes/.

bash
Copy code
grep -Ri "http" frontend/src/app/
This reveals endpoints like:

bash
Copy code
http://localhost:3000/ftp/acquisitions.md
Look for payload examples, token generation, or even hardcoded secrets.

📎 Step 6: Map & Reconstruct the Hidden Endpoint
From earlier results (e.g., /ftp/acquisitions.md), build the URL:

bash
Copy code
http://localhost:3000/ftp/acquisitions.md
Access it via browser or terminal:

bash
Copy code
curl http://localhost:3000/ftp/acquisitions.md
🎯 Goal: Validate if the file is exposed. You should receive HTTP 200 OK with the content.

🧠 Technical Strategy Behind This Method
Core Idea:
Static code analysis is a primary technique in white-box penetration testing where attackers have full or partial access to source code. It bypasses the limitations of black-box testing by exposing logic errors, insecure configurations, and hidden paths before they manifest as runtime behavior.

Key Concepts Used:

Information Disclosure: Unintended exposure of files due to improper access control.

Express Static Middleware: A method in Node.js Express framework to expose files — often misconfigured.

sendFile() Misuse: Hardcoded file paths can become access points if not secured with authorization middleware.

Lack of Access Controls: No authentication/authorization check on sensitive routes.

🔐 Why We Chose Source Code Review
Maximum Coverage: Ensures no blind spots compared to scanning tools.

Deterministic Results: Every line of code is reviewed systematically.

Bypasses Obfuscation: Front-end protections or route hiding have no effect.

Blueprint for Custom Exploits: Perfect for developing PoCs (Proof of Concepts).

🧩 Final Notes
Always cross-check source-discovered paths with actual HTTP responses.

Combine static analysis with dynamic techniques (e.g., FFUF, DevTools) to confirm impact.

Automate with tools like semgrep, grep, AST analyzers for faster reviews on large apps.

🧪 Example Outcome
You discover this:

js
Copy code
app.use('/ftp', express.static(path.join(__dirname, '../ftp')))
You visit:

bash
Copy code
http://localhost:3000/ftp/acquisitions.md
✅ File is accessible — Challenge Solved.
