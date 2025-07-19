# 🕵️ OWASP Juice Shop – Confidential Document Challenge
🧠 OWASP Juice Shop - OffensiveThis README acts as a master walkthrough for solving challenges in the OWASP Juice Shop using various offensive methodologies. It provides deep technical explanations for each method and each command used, from reconnaissance to exploitation.

📁 Folder Structure Overview

```
OWASP-Juice-Shop-Offensive-Security-Toolkit/
├── Method-01_Devtools-Network-Trick/
├── Method-02_Direct-API-Call/
├── Method-03_Modify-URL-Manually/
├── Method-04_Automated-Tools/
├── Method-05_Obfuscated-Content-Reveal/
├── Method-06_HTML-Comment-Backdoor/
├── Method-07_Form-Field-Manipulation/
├── Method-08_Client-Side-Source-Analysis/
├── Method-09_CSP-Bypass-Error-Stacktrace/
├── Method-10_Error-Page-Hint/
├── Method-11_Path-Disclosure-Debug/
├── Method-12_Admin-Debug-Path-Leak/
├── Method-13_JS-Deobfuscation-Variable-Recon/
├── Method-14_JS-Fetch-Injection/
└──
```
Each folder contains:

README.md → Challenge description, technical methodology, backend rationale

TODO.md → A highly detailed, step-by-step task list for execution

✅ GOAL

To demonstrate and practice modern offensive security techniques used in real-world reconnaissance, client-side exploitation, privilege escalation, and unauthorized access via misconfigurations and insecure JS logic.

This is not CTF-style; rather, this is real-world attacker emulation with deep technical understanding.

🧩 MODULE EXAMPLE - Method-01_Devtools-Network-Trick/

🔍 Objective:

Access hidden functionality by intercepting network calls using Developer Tools.

🛠️ Technical Strategy:

Modern web apps use asynchronous requests (AJAX / fetch) to retrieve sensitive information. These requests often don't reflect directly in the browser UI but are still accessible via the Network tab.

🧪 Steps:

Open Juice Shop in your browser (default: http://localhost:3000).

Open DevTools → F12 or Ctrl+Shift+I → Network tab.

Click a suspicious link or navigate between sections.

Observe XHR / fetch calls made in the background (e.g. /rest/user/whoami).

Right-click → Open in new tab or copy the URL.

Paste in browser → check if data is shown directly.

🔬 Backend Explanation:

These fetch requests often interact with REST APIs exposed by the app. The REST endpoints may not require strict auth headers or role validation. If a browser fetches it, you can too (unless protected).

📌 Example:

GET /rest/admin/application-configuration

Expected Output: JSON configuration object

If exposed, reveals debug flags, email creds, or logging levels.

📦 TOOLKIT METHOD - Method-13_JS-Deobfuscation-Variable-Recon/

🔍 Objective:

Understand and extract hardcoded secrets or logic hidden inside obfuscated JavaScript.

🛠️ Technical Strategy:

Obfuscation is used to hide critical logic or variables from users. But since JS is client-side, you can beautify, trace, and reverse this logic.

🧪 Steps:

Open DevTools → Sources tab → locate main.*.js

Right-click → "Pretty Print" {} (beautifies minified JS)

Search for keywords: secret, key, config, token, fetch

Observe variable names and structure → trace how values are computed

Log variables in the browser console:

console.log(secretToken)

If it's used in a fetch call, replace it with your payload:

fetch("/api/admin?token=" + secretToken)

🔬 Backend Explanation:

Developers sometimes hardcode environment secrets, admin flags, or debugging bypass tokens into JS during development and forget to strip them during production. These can be directly exploited.

🔐 AUTH BYPASS - Method-12_Admin-Debug-Path-Leak/

🔍 Objective:

Use leaked paths from debug logs or error stacks to access internal admin routes.

🛠️ Technical Strategy:

Some JS apps expose stack traces or 404s that leak internal file names or endpoints. If you spot something like:

Error at /app/routes/admin/debugPanel.js

You can try visiting /admin/debugPanel directly.

🧪 Steps:

Force an error (e.g., wrong input, tamper requests)

Check response body for stack traces or file paths

Look for paths like /admin/*, /internal/, /debug/

Try accessing those directly → /#/admin/debug

🔬 Backend Explanation:

Many modern frameworks (Node.js, Express, Angular) map file structure to routes. Stack trace or exception handlers might expose these, revealing unauthenticated or legacy paths.

🛡️ WHY THIS MATTERS (Offensive Security Lens)

Web apps are uncompiled and fully client-visible.

Client-side JavaScript is never secure from attackers.

Every fetch, form, endpoint, or DOM element is an attack vector.

This repo trains you in browser-native red teaming.

You're not just solving a CTF. You’re:

Practicing bug bounty reconnaissance

Learning real-world JS source tracing

Understanding auth bypass via broken access control

Simulating post-exploitation enumeration

🧠 Final Tips

Use Burp Suite to capture and modify requests

Try automation with Puppeteer, Playwright, or Selenium

Log all variables and deobfuscate slowly

Always check JS, HTML comments, error messages
