✅ OWASP-Juice-Shop-Admin-Debug-Path-Leak/TODO.md
markdown
Copy code
# ✅ TODO - Admin Debug Path Leak

## 🎯 Goal
Find and exploit internal debug/admin paths exposed via verbose error messages or misconfigurations in OWASP Juice Shop.

---

## 🛠️ Manual Recon Method
- [ ] 🚀 Start Juice Shop in Docker
    ```bash
    docker run -d -p 3000:3000 --name juice-shop bkimminich/juice-shop
    ```
- [ ] 🌐 Navigate to: http://localhost:3000
- [ ] 💥 Manually hit a broken URL (e.g., `/breakme`, `/notarealpage`) to trigger errors
- [ ] 🔍 Look for debug traces, stack paths, or dev-only endpoints in the browser/devtools

---

## 🧪 Automated Enumeration (Kali Linux)
- [ ] 🕵️ Use `ffuf` or `dirb` to brute-force hidden paths
    ```bash
    ffuf -u http://localhost:3000/FUZZ -w /usr/share/wordlists/dirb/common.txt
    ```
- [ ] 🔍 Inspect results for:
    - `/debug`
    - `/rest/admin`
    - `/internal/logs`
    - `/api/debug/status`

---

## 🧬 Advanced Exploit (Optional)
- [ ] Use `curl` or `httpie` to probe endpoints:
    ```bash
    curl -I http://localhost:3000/debug
    ```
- [ ] Try parameter fuzzing like:
    ```
    /debug?env=dev
    /debug?token=xyz
    ```

---

## 🧯 Mitigation Notes (Document Later)
- [ ] Enforce `NODE_ENV=production`
- [ ] Turn off stack trace display
- [ ] Block sensitive endpoints with auth guards

---

## 📸 Proof
- [ ] Screenshot the leaked debug path or internal route trace
- [ ] Add notes on exploitation steps taken

---
