📁 Method-09_Wayback-or-Cache-Recon/TODO.md
markdown
Copy code
# TODO - Method 09: Wayback Machine or Cache-Based Reconnaissance

## 🎯 Objective
Utilize internet archives like the Wayback Machine and cached pages to uncover old, hidden, or sensitive endpoints that may expose private files, APIs, or features previously part of the application.

## 🛠️ Tools Required
- `waybackurls` (Go tool)
- `gau` (GetAllUrls)
- `curl` (for validation)
- `httpx` (optional: to probe for live endpoints)
- Internet connection for archive fetching

## 🧪 Step-by-Step Task List

### ✅ 1. Install Necessary Tools
- [ ] Ensure `waybackurls` is installed:  
  ```bash
  go install github.com/tomnomnom/waybackurls@latest
 Install gau (GetAllUrls):

bash
Copy code
go install github.com/lc/gau/v2/cmd/gau@latest
✅ 2. Setup Recon Directory
 Create a working folder and switch into it:

bash
Copy code
mkdir WaybackRecon && cd WaybackRecon
✅ 3. Extract URLs Using Wayback Machine
 Run waybackurls to fetch historical URLs:

bash
Copy code
echo "https://juice-shop.example.com" | waybackurls > wayback-urls.txt
✅ 4. Extract URLs Using gau
 Run gau for another layer of URL extraction:

bash
Copy code
echo "juice-shop.example.com" | gau > gau-urls.txt
✅ 5. Combine and De-duplicate
 Merge and sort:

bash
Copy code
cat wayback-urls.txt gau-urls.txt | sort -u > all-archived-urls.txt
✅ 6. Filter Interesting URLs
 Grep for files of interest like .js, .php, .json, etc:

bash
Copy code
grep -E "\.js|\.php|\.json|\.bak|\.env" all-archived-urls.txt > interesting-urls.txt
✅ 7. Validate Live Endpoints (Optional)
 Use httpx to probe live URLs:

bash
Copy code
cat interesting-urls.txt | httpx -silent -status-code -title
✅ 8. Manual Review
 Review JS files for hardcoded secrets, API keys

 Identify sensitive endpoints previously accessible

 Document findings

📌 Notes
Validate findings via browser or curl:

bash
Copy code
curl -I <URL>
Watch for 200/403/500 codes indicating sensitive or broken access.

✅ 9. Save Logs & Outputs
 Create a structured logs/ directory

 Save raw outputs and highlight any vulnerable or interesting endpoints manually.

🔐 Goal
To uncover potentially forgotten or deprecated endpoints that were once public, using passive reconnaissance techniques without alerting the target.
