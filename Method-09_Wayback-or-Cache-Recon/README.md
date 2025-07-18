📁 OWASP-Juice-Shop-Confidential-Document-Challenge/Method-09_Wayback-or-Cache-Recon/README.md
markdown
Copy code
# 🧠 Method-09: Wayback Machine & Cache Recon for Hidden File Discovery

## 🎯 Objective
Leverage public web archival and caching services like **Wayback Machine (archive.org)** and **Google Cache** to identify **previously indexed but now-hidden** or sensitive documents (e.g., PDFs, internal reports) associated with the OWASP Juice Shop instance. These may expose confidential information and can help complete the challenge.

---

## 📚 Background & Methodology

### 🔍 What is the Wayback Machine?
The **Wayback Machine** is a digital archive maintained by [archive.org](https://archive.org). It periodically snapshots and stores publicly accessible web pages. Even if a file or page is deleted from the current web server, it might still be **archived** and publicly available via this service.

### 🕵️ Why use this for Offensive Security?
Files that are **accidentally exposed** and later removed may still exist in public caches or archives. Attackers use this method in **OSINT (Open Source Intelligence)** gathering and **passive reconnaissance** to uncover historical leaks or sensitive endpoints.

---

## ⚙️ Tools & Requirements

- 🐧 Kali Linux or any Linux distro
- 🔧 `waybackurls` (part of the Go recon toolset)
- 🧰 `gau` (GetAllUrls – tool to fetch URLs from various sources including the Wayback Machine, Common Crawl, etc.)
- 🔍 Browser for manual cache inspection (optional)

---

## 🧪 Step-by-Step Instructions

### ✅ Step 1: Install Required Tools

```bash
go install github.com/tomnomnom/waybackurls@latest
go install github.com/lc/gau@latest
export PATH=$PATH:$(go env GOPATH)/bin
✅ Step 2: Harvest Historical URLs for Juice Shop
Replace <TARGET_DOMAIN> with the Juice Shop’s IP or domain (e.g., localhost:3000, or the actual exposed domain if public)

bash
Copy code
echo "<TARGET_DOMAIN>" | waybackurls | tee wayback-output.txt
🔍 Explanation:

waybackurls queries the Wayback Machine for all known URLs associated with the target domain.

Output is saved into wayback-output.txt.

Expect output like:

bash
Copy code
http://localhost:3000/docs/internals.pdf
http://localhost:3000/secret/README_old.html
🧠 Technical Note: The Wayback Machine returns a list of historical snapshots of public files — some of which may no longer exist on the live server but are cached.

✅ Step 3: Use gau to get broader cached results
bash
Copy code
echo "<TARGET_DOMAIN>" | gau --subs | tee gau-output.txt
🔍 Explanation:

gau aggregates URLs from various sources: Wayback Machine, Common Crawl, AlienVault OTX, etc.

--subs tells gau to include subdomains if available.

Compare results with wayback-output.txt to find more sensitive or hidden files.

✅ Step 4: Filter for Likely Sensitive Files
bash
Copy code
cat wayback-output.txt gau-output.txt | sort -u | grep -Ei "\.(pdf|docx?|xlsx?|txt|bak|zip|tar|gz|conf|config|json|xml)$" > sensitive-files.txt
🔍 Explanation:

You're looking for file extensions commonly linked to configuration, internal documentation, or backups.

You might find something like:

bash
Copy code
http://localhost:3000/uploads/backup-config.zip
http://localhost:3000/secrets/flag.pdf
✅ Step 5: Access the Archived Version (If 404 on Live Site)
If a URL from Wayback exists but returns 404 live, manually check it on:

📌 https://web.archive.org/web/*/http://localhost:3000/secrets/flag.pdf

👨‍💻 Optional - curl the Wayback Archive copy directly:

bash
Copy code
curl -I "https://web.archive.org/web/*/http://<TARGET>/somefile.pdf"
🎯 Final Goal
Find a PDF or similar file that:

Was previously available but is now hidden

Can still be retrieved via archived copy

Contains a flag, confidential report, or internal document

🛡️ Technical Strategy Summary
Technique	Purpose
Wayback URLs	Recover removed or hidden URLs via historical snapshots
GAU Tool	Get broader view across various URL aggregators
Passive Recon	Zero-impact information collection (no active probing or port scanning)
File Heuristics	Filter by extension to guess likely sensitive documents

🔐 Why This Method Works (Offensive Security Context)
Developers often remove files too late after being indexed.

Archived pages are often ignored during cleanup.

Recon using public caches requires no authentication or intrusion, making it low-risk and effective.

This method is widely used by bug bounty hunters, pen testers, and threat actors during reconnaissance and is often the first step in digital footprinting.

📝 Pro-Tip for Enhancement
You can automate validation of URLs using httpx to check for status codes and content types:

bash
Copy code
cat sensitive-files.txt | httpx -status-code -title -content-type
This helps confirm which files still exist or serve interesting content.

✅ Conclusion
This method is critical for catching historically exposed but currently unavailable documents. Use it early in the recon phase and combine with other heuristics (robots.txt, dirsearch, etc.) to build a full picture of the application’s attack surface.
