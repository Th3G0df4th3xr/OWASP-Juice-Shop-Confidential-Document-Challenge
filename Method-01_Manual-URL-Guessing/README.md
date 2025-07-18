📄 /Method-01_Manual-URL-Guessing/README.md
markdown
Copy code
# 🛠️ Manual URL Guessing – OWASP Juice Shop: Confidential Document Challenge

## 🎯 Objective

Manually discover and access a sensitive document (`legal.md`) hosted in the `/ftp/` directory of the OWASP Juice Shop application to trigger the “Confidential Document” challenge.

---

## 🧠 Strategy & Methodology

This method simulates a **manual path enumeration attack**, commonly used in real-world penetration tests to discover **unlinked or hidden files** on a web server. Such files may contain sensitive information and are often placed in accessible directories without proper access control mechanisms.

### Why This Works

The Juice Shop intentionally leaves a Markdown file in an exposed directory (`/ftp/`) without authentication or obfuscation. Since the file is not linked from the UI, discovering it requires either:

- **Brute-force enumeration**
- **Prior knowledge or logical guessing**

This mimics real-life **Information Disclosure vulnerabilities**, where sensitive files are mistakenly left accessible.

---

## ⚙️ Technical Flow of the Attack

We’ll:
1. Discover the `/ftp/` directory structure manually via browser or terminal.
2. Guess file names based on logical reasoning.
3. Validate our guesses using HTTP responses.

---

## 🔐 Backend Concepts Involved

- **HTTP GET Requests**: Each URL we access triggers a GET request to the server asking for the resource.
- **Directory Traversal (Not Used Here)**: Although not applicable in this method, the logic is similar — accessing files through improper controls.
- **Status Codes**: HTTP 200 = OK (file exists), 404 = Not Found (wrong guess).

---

## 🧪 Step-by-Step Execution (Manual Recon Method)

### ✅ Step 1: Confirm Juice Shop is Running

If you're running Juice Shop locally via Docker:

```bash
docker run -d -p 3000:3000 --name juice-shop bkimminich/juice-shop
This pulls and starts Juice Shop container.

It maps container port 3000 to localhost 3000.

💡 Access the app at:

arduino
Copy code
http://localhost:3000
✅ Step 2: Manually Guess and Visit Known Static Paths
Open your browser and visit:

url
Copy code
http://localhost:3000/ftp/legal.md
🎯 This path is logically guessable based on Juice Shop’s theme and folder naming conventions.

📬 What Happens Behind the Scenes:
When you visit /ftp/legal.md, your browser sends an HTTP GET request to:

vbnet
Copy code
GET /ftp/legal.md HTTP/1.1
Host: localhost:3000
If the file exists, the server returns:

pgsql
Copy code
HTTP/1.1 200 OK
Content-Type: text/markdown
Your browser renders the Markdown content, and Juice Shop internally flags the challenge as solved using client-side challenge tracking logic (Angular-based frontend calls the challenge-checker API).

✅ Step 3: Observe the Trigger
If you guessed the file correctly (legal.md), you will see:

A rendered Markdown document in your browser.

A toast notification (green box) from Juice Shop saying:

“Challenge solved: Confidential Document”

💡 Tips for Logical Path Guessing
Always think like a developer:

Files like legal.md, terms.md, privacy.pdf are common.

Directories like /ftp/, /documents/, /static/ are used to host such files.

Try URLs such as:

text
Copy code
/ftp/confidential.md
/ftp/acquisition.md
/ftp/legal.pdf
/ftp/statement.txt
Use browser history, developer intuition, and naming conventions.

🔍 Technical Notes
Term	Explanation
Markdown (.md)	A lightweight markup language used for documentation. Server serves it as text/plain or text/markdown
HTTP 200 OK	Response code indicating the file was successfully found and returned
Path Enumeration	Process of trying to guess valid URLs or endpoints on a web server
Client-side challenge validation	Juice Shop uses Angular to check whether a challenge has been solved based on user actions

🛡️ Real-World Risk Mapping
This mirrors vulnerabilities like:

Insecure Direct Object Reference (IDOR)

Information Disclosure due to Poor Access Controls

Unlinked Resource Exposure

🧠 Why This Method Is Still Relevant
Many developers assume “security through obscurity” — hiding files without proper authorization. This manual testing method exposes such risky assumptions.

📌 Summary
Step	Action
1	Run Juice Shop and access /ftp/legal.md
2	Trigger the HTTP GET request
3	Observe the returned document
4	Confirm challenge is solved

✅ Result
🎉 Challenge marked as solved once the file is accessed!

yaml
Copy code

---
