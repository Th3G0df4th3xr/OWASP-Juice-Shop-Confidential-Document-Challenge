📁 OWASP-Juice-Shop-Confidential-Document-Challenge/Method-01_Manual-URL-Guessing/TODO.md
markdown
Copy code
# ✅ TODO - Manual URL Guessing Approach (Method 01)

This file outlines a structured set of tasks required to execute and document the Confidential Document Challenge using Manual URL Guessing. Each step reflects a logical action in the offensive security methodology.

---

## 🛠️ Step-by-Step Tasks

### [ ] 1. Set Up Juice Shop Instance (Locally via Docker)
- Command:
  ```bash
  docker run -d -p 3000:3000 --name juice-shop bkimminich/juice-shop
Expected: Juice Shop should be running on http://localhost:3000

[ ] 2. Open Developer Tools in Browser
Action: Press F12 or Ctrl+Shift+I in browser

Goal: Inspect HTTP requests, explore directory structures

[ ] 3. Manually Access Suspicious URLs
Try these endpoints:

plaintext
Copy code
http://localhost:3000/ftp/
http://localhost:3000/ftp/legal.md
http://localhost:3000/ftp/acquisitions.md
Expected: If successful, you will be able to read confidential markdown files.

[ ] 4. Explore robots.txt or .well-known/security.txt (Optional Discovery Step)
Try:

plaintext
Copy code
http://localhost:3000/robots.txt
http://localhost:3000/.well-known/security.txt
Insight: Files here may leak paths or intentions to restrict crawling, hinting toward hidden folders like /ftp/.

[ ] 5. Analyze Directory Behavior
Observation:

Attempt directory traversal by trimming segments from a known document URL.

Understand how the Juice Shop exposes static files under /ftp/.

[ ] 6. Document Why This Works
Write about how improperly secured directories lead to Insecure Direct Object References (IDOR).

Explain how static folder exposure enables confidential data exposure.

[ ] 7. Record Evidence for Report
Take:

Screenshots of the documents accessed.

Browser network logs showing 200 OK status.

[ ] 8. Write Final Exploit Summary in README.md
Include:

All steps you performed

Observations

Risk implications

Recommended remediations (e.g., access controls, disallowing directory listing)

[ ] 9. Commit & Push to GitHub
Command:

bash
Copy code
git add .
git commit -m "Added Manual URL Guessing Method for Confidential Document Challenge"
git push origin main
📌 Notes
This method does not use any tool—pure manual enumeration.

It tests for human error in path protection.

You are mimicking what a real-world recon attack phase might look like using basic OSINT + brute-pathing.
