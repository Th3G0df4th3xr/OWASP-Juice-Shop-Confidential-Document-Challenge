📁 OWASP-Juice-Shop-JS-Deobfuscation-Variable-Recon/README.md
markdown
Copy code
# 🧠 JS Deobfuscation & Variable Recon - OWASP Juice Shop

## 🎯 Objective
Reverse engineer obfuscated JavaScript files from OWASP Juice Shop to uncover hidden variables, secrets, or endpoint references used in sensitive logic such as authentication, admin paths, or security bypass mechanisms.

---

## 🛠️ Why This Works

Modern web applications often rely on obfuscated JavaScript to make reverse engineering difficult. However:
- JavaScript executes on the client-side.
- Obfuscation ≠ encryption.
- Static analysis and runtime debugging can decompile and analyze scripts.
We exploit this to **reveal internal logic, secrets, API calls, tokens, and hidden paths**.

---

## 🚀 Step-by-Step Attack Strategy

### 1. 🧼 Clean Slate: Start Juice Shop
```bash
docker run -d -p 3000:3000 --name juice-shop bkimminich/juice-shop
Open http://localhost:3000 in your browser.

2. 🕸️ Identify JavaScript Assets
In Chrome DevTools (F12):

Go to Sources > js/ or webpack:// under the left-side project navigator.

Locate files like:

main.*.js

runtime.*.js

vendor.*.js

3. 🔍 Look for Obfuscation
Look for symptoms of obfuscation:

Single-letter variables (e.g., a = 0; b = 1;)

Function chains like _0xabc123[0]

Hexadecimal strings like _0x4f2b[0x12a]

4. 🛠️ Deobfuscate the Code
Option A: Use DevTools Live
Right-click → “Format (Pretty Print)” the JS file

Set breakpoints on suspect functions

Watch variable values at runtime

Use console.log() in browser console to print obfuscated variable results

Option B: Use Online Tools
Copy-paste obfuscated JS to:

https://lelinhtinh.github.io/de4js/

Select Packer, Obfuscator, JavascriptObfuscator options

5. 🔎 Variable Recon
You are specifically looking for:

Secret tokens

Internal route strings (e.g., /adminpanel)

API endpoints

Auth-related variable names (e.g., isAdmin, authToken)

Base64/Hex-encoded values

Example:

js
Copy code
var _0xabc12 = '/rest/admin/debug';
May reveal hidden REST endpoints.

6. 🧬 Runtime Exploration
Use DevTools Console to inspect runtime values:

js
Copy code
console.log(_0xabc12);
Dump the entire object or function:

js
Copy code
console.dir(Object.keys(window))
Trace obfuscated function execution:

js
Copy code
debugger;
🧠 Technical Concepts Involved
Obfuscation: Code transformation to reduce readability (not encryption)

Pretty Print: Reformats minified JS to human-readable format

Breakpoint Injection: Halts execution to inspect runtime behavior

Static vs. Dynamic Analysis: Reading source vs executing to understand flow

Memory Inspection: Watching variable states live in browser

📸 Proof of Work
 Screenshot of obfuscated JS

 Screenshot of deobfuscated logic showing sensitive variable

 Captured payload or internal path

🛡️ Remediation
Avoid exposing sensitive paths in JS

Minify AND encrypt sensitive tokens

Use server-side validation (not just client-side)

🧰 Optional Tools
Tool	Use
source-map-explorer	Analyze bundled JS
js-beautify (CLI)	Format minified code
obfuscator.io	Analyze obfuscation patterns
Burp Suite	Monitor JS-triggered calls
