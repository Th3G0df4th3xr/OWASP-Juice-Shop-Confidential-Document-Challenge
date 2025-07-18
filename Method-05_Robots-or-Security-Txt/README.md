📁 Method-05_Robots-or-Security-Txt/README.md
markdown
Copy code
# 🕵️‍♂️ Method 05 - robots.txt / security.txt Enumeration (Information Disclosure)

## 🎯 Objective:
Leverage misconfigurations or overlooked security practices via `robots.txt` or `.well-known/security.txt` to enumerate sensitive hidden files or directories — potentially leading to unauthorized access to confidential documents.

---

## 🛠️ Tools & Environment:
- Kali Linux / Parrot OS / PentesterLab VM
- OWASP Juice Shop running locally (`http://localhost:3000`)
- Terminal (bash/zsh)
- `curl`, `wget`, `cat`, or web browser

---

## 🧪 Step-by-Step Methodology:

### 🔍 Step 1: Manual Discovery via Browser

Open your browser and visit the following URLs:

```http
http://localhost:3000/robots.txt
http://localhost:3000/.well-known/security.txt
🎯 These files often provide directives for crawlers (robots.txt) and contact/security policies for security researchers (security.txt).

🧠 What Happens in the Backend?
robots.txt is parsed by web crawlers to determine which paths should not be indexed.

It is not a security mechanism — attackers often use it to find sensitive endpoints.

security.txt is a voluntary standard (RFC 9116) to disclose security contact info — but may unintentionally leak internal documentation URLs or directory structures.

🧾 Step 2: Use curl or wget to Retrieve the Files
robots.txt
bash
Copy code
curl http://localhost:3000/robots.txt
Expected Output:

makefile
Copy code
User-agent: *
Disallow: /ftp
Disallow: /confidential.pdf
security.txt
bash
Copy code
curl http://localhost:3000/.well-known/security.txt
Expected Output (example):

pgsql
Copy code
Contact: security@juice-shop.com
Policy: http://localhost:3000/security-policy.pdf
🔍 Step 3: Analyze the Disallowed or Mentioned URLs
From the Disallow: or file references:

Try directly visiting:

http://localhost:3000/ftp/

http://localhost:3000/confidential.pdf

http://localhost:3000/security-policy.pdf

✅ You may uncover sensitive documents like internal policies, user data, or challenge flags.

🧠 Technical Rationale
robots.txt is a high-value passive recon target.

It reveals attacker-worthy URLs under the assumption that "security by obscurity" will protect them — this is a false assumption.

Security.txt, although helpful in disclosure practices, can unintentionally link to sensitive documentation or staging environments.

💥 Bonus – Automate the Scan (Optional)
Use dirb, gobuster, or feroxbuster with wordlists including:

bash
Copy code
robots.txt
security.txt
ftp
confidential.pdf
internal/
hidden/
Example with dirb:

bash
Copy code
dirb http://localhost:3000/ /usr/share/wordlists/dirb/common.txt
🧷 Conclusion
This technique exploits poor information hiding mechanisms where sensitive documents are exposed by being referenced in crawler policy files. These should never include sensitive paths unless they're also protected with authentication and authorization mechanisms.

🗂️ Artifacts to Capture:
Screenshot of the robots.txt or security.txt contents

Screenshot of the confidential file accessed

Log of curl/wget commands used

Short PoC write-up showing how enumeration led to unauthorized document access

✅ Status:
 Initial Enumeration

 Identified Disallowed/Linked Files

 Accessed and Validated Confidential Documents

 Documented PoC with Screenshots

yaml
Copy code
