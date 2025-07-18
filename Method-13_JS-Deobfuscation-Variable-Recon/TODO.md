🧪 Method-13_JS-Deobfuscation-Variable-Recon/TODO.md
markdown
Copy code
# ✅ TODO - JS Deobfuscation & Variable Recon Method

## 🔍 Objective
Identify hidden functionality or leaked paths/endpoints through obfuscated JavaScript (JS) code used in OWASP Juice Shop.

---

## 🛠️ Steps to Perform

### 1. Locate Obfuscated JS File
- [ ] Open Developer Tools → Sources tab.
- [ ] Look for JS files like `main.*.js`, `runtime.*.js`, or any file with minified or non-readable names.
- [ ] Save or copy the contents of obfuscated JS.

### 2. Deobfuscate JavaScript
- [ ] Use tools:
  - [ ] Browser-based: [Prettier.io](https://prettier.io/playground/)
  - [ ] CLI: `js-beautify` — `npm install -g js-beautify`
    - Run: `js-beautify -r <filename>.js`
- [ ] Make code readable (check for `eval()`, `atob()`, `setTimeout()` with function strings, etc.)

### 3. Recon for Variables and Paths
- [ ] Search for variables like `admin`, `debug`, `path`, `api`, `secret`, `scoreboard`, etc.
- [ ] Observe URL fragments, internal endpoints, flag locations.
- [ ] Look for any use of `localStorage`, `sessionStorage`, or cookies.

### 4. Analyze Function Usage
- [ ] Find how the variables and paths are being used.
- [ ] If dynamic routes are revealed (e.g., `/rest/debug`, `/api/scoreboard`), try accessing them directly.

### 5. Cross-Verify in App
- [ ] Try direct navigation or API requests via browser or `curl`:
  - `curl http://<juice-shop-ip>:3000/<leaked-path>`

### 6. Solve the Challenge (if any)
- [ ] If challenge-specific, visit or trigger leaked path/variable function to mark it complete.

---

## 🧠 Pro Tips
- Use breakpoints to pause JS execution and observe values.
- If obfuscated using `eval(atob(...))`, decode base64 and review manually.
- Watch for inline JSON structures with config or credentials.

---

## 📁 Artifacts
- [ ] Save the prettified JS for reference.
- [ ] Capture screenshots of leaked variables or endpoints.
