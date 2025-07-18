📄 Method-04_Source-Code-Review/TODO.md
markdown
Copy code
# ✅ TODO - Source Code Review Method (Confidential Document Challenge)

## 📦 1. Clone the OWASP Juice Shop Source Code
- [ ] Download the Juice Shop repo from GitHub:
  ```bash
  git clone https://github.com/juice-shop/juice-shop.git
 Move into the source directory:

bash
Copy code
cd juice-shop
🕵️‍♂️ 2. Search for Sensitive Keywords
 Grep through source code for keywords:

bash
Copy code
grep -RiE 'confidential|admin|secret|document|hidden|file|flag|leak' .
🔎 3. Examine Routes and Static Assets
 Inspect files under routes/:

Focus on app.js, server.js, or routes/*.js

Check for routes rendering .pdf, .doc, or .html documents.

 Check frontend/src/app/ for hidden UI paths or logic.

🧪 4. Look for Unlinked or Hidden Features
 Identify any HTTP GET or POST routes not linked from UI

 Review src/assets/public or static files folder

 Identify documents not served via frontend routes

🧠 5. Analyze for Authorization Logic
 Check if any route is missing auth middleware

Look for lack of checks like verifyAccessToken or isAuthorized

 Note exposed endpoints returning confidential data

📤 6. Test Suspected Endpoints in Browser or Burp Suite
 Manually trigger routes found via source inspection

 Check response for document file types like PDF, DOCX, TXT

🗂️ 7. Document All Findings
 Add screenshots and endpoint URLs

 Note how source code led to discovery

 Log path traversal or disclosure risk, if found

📚 8. Update README with Steps and Screenshots
 Mention exact lines where logic leak is identified

 Provide justification for choosing source-based analysis

🧠 Tip: Try combining source analysis with runtime browser tools to confirm suspicions. Always verify if discovered documents can be accessed without auth.
