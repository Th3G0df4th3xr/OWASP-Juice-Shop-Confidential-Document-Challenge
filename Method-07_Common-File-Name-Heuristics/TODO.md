📝 OWASP-Juice-Shop-Confidential-Document-Challenge/Method-07_Common-File-Name-Heuristics/TODO.md
markdown
Copy code
# ✅ TODO - Common File Name Heuristics Attack

## 🧰 Setup Environment
- [ ] Export target URL (e.g., `http://localhost:3000`)
- [ ] Ensure `ffuf`, `gobuster`, or `dirsearch` is installed

---

## 📂 Wordlist Preparation
- [ ] Choose a high-quality wordlist:
  - [ ] `raft-large-files.txt`
  - [ ] `SecLists/Fuzzing/filename-predictable.txt`
- [ ] Include common extensions:
  - [ ] `.zip`, `.bak`, `.old`, `.tar.gz`, `.txt`, `.conf`, `.pdf`

---

## 🚀 Launch Fuzzing
- [ ] Run `ffuf` with extensions:
  ```bash
  ffuf -w /path/to/wordlist -u $TARGET/FUZZ -e .bak,.zip,.old,.tar.gz,.conf,.pdf -mc 200,403,401 -t 50 -v
🔍 Review Results
 Look for files returning:

 200 OK — Directly downloadable

 403 Forbidden — Try header manipulation

 Manually visit interesting file paths

 Inspect content for:

 Leaked credentials

 Sensitive internal info

 Configuration files

🧪 Post-Processing
 Use file, strings, binwalk, or unzip to extract content

 Document which file revealed the flag or confidential document

🧾 Documentation
 Save screenshots of terminal output and downloaded files

 Log vulnerable file paths and response codes

 Write a brief summary of this method’s effectiveness

🧠 Optional Enhancements
 Use Burp Suite Intruder with custom filename payloads

 Automate detection with a bash script for multiple extensions

vbnet
Copy code
