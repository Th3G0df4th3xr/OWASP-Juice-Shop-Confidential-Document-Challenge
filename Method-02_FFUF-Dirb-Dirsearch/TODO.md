📁 OWASP-Juice-Shop-Confidential-Document-Challenge/Method-02_FFUF-Dirb-Dirsearch/TODO.md
markdown
Copy code
# ✅ TODO - FFUF / Dirb / Dirsearch Method

## 🔧 1. Setup & Prepare Environment
- [ ] Ensure OWASP Juice Shop is running:
  ```bash
  docker run -d -p 3000:3000 --name juice-shop bkimminich/juice-shop
 Confirm it's accessible at http://localhost:3000

⚔️ 2. Run FFUF
 Use a strong wordlist (e.g., /usr/share/wordlists/dirb/common.txt)

 Execute:

bash
Copy code
ffuf -u http://localhost:3000/FUZZ -w /usr/share/wordlists/dirb/common.txt -t 40 -o ffuf_results.json
 Review for interesting files (.md, .zip, .bak, /ftp/, /backup/, etc.)

🕵️ 3. Run Dirb (Alt Method)
 Execute:

bash
Copy code
dirb http://localhost:3000 /usr/share/wordlists/dirb/common.txt
 Look for: /ftp/, /confidential/, /documents/, etc.

🧭 4. Run Dirsearch (Python Alternative)
 Clone if not already installed:

bash
Copy code
git clone https://github.com/maurosoria/dirsearch.git
 Execute:

bash
Copy code
python3 dirsearch/dirsearch.py -u http://localhost:3000 -e txt,md,js,json,zip,bak -x 404
🕸️ 5. Post-Discovery
 Try visiting found paths in browser or curl:

bash
Copy code
curl http://localhost:3000/ftp/acquisitions.md
 Download any documents found for analysis

🧪 6. Analyze Content
 Check for:

Company secrets

Acquisition plans

Legal or internal docs

 Capture screenshots or copy content for report

📜 7. Document Findings
 Create a findings.md and note:

URLs found

Content summary

Exploitation potential

🔐 8. Suggest Remediations
 Deny public access to sensitive files

 Use auth & permission-based control

 Disable directory indexing

yaml
Copy code
