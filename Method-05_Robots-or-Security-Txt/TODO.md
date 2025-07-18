📁 OWASP-Juice-Shop-Confidential-Document-Challenge/Method-05_Robots-or-Security-Txt/TODO.md
markdown
Copy code
# ✅ TODO - robots.txt & security.txt Enumeration Method

## [ ] Step 1: Start the OWASP Juice Shop Container
- [ ] Use Docker or local instance to run Juice Shop on `http://localhost:3000`

## [ ] Step 2: Enumerate robots.txt
- [ ] Open terminal and run:
  ```bash
  curl -I http://localhost:3000/robots.txt
 If 200 OK, fetch the content:

bash
Copy code
curl http://localhost:3000/robots.txt
 Note any Disallow entries and explore them manually.

[ ] Step 3: Check for security.txt
 In terminal:

bash
Copy code
curl http://localhost:3000/.well-known/security.txt
 If present, examine for any links, emails, or directories that could help.

[ ] Step 4: Investigate Hidden Paths
 Explore hidden endpoints or documentation mentioned.

 Access them in the browser or using curl.

[ ] Step 5: Document Your Findings
 Note down discovered hidden paths.

 Correlate with potential sensitive files (e.g., /ftp, /confidential, etc.)

[ ] Step 6: Capture Screenshots
 Document any successful access to confidential pages.

[ ] Step 7: Mitigation & Defense
 Note that robots.txt should not be used to protect sensitive files.

vbnet
Copy code
