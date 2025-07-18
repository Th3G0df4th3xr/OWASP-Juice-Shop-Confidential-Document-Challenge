📁 /Method-14_JS-Fetch-Injection/
🧠 Challenge Context
In OWASP Juice Shop, certain JavaScript files invoke internal API endpoints via fetch() calls. These calls can sometimes be manipulated (or injected) through browser developer tools to bypass client-side restrictions or tamper with the request payload.

The goal here is to intercept or replicate a vulnerable fetch() call, tamper with it, and observe server behavior — potentially triggering an internal or unauthorized functionality.

📄 README.md
markdown
Copy code
# 🍯 Method-14: JS Fetch Injection

## 🎯 Objective
Exploit a `fetch()`-based JavaScript request to gain access to internal functionality or trigger a vulnerable path by injecting into the `fetch()` method. This method focuses on understanding how the browser handles JavaScript fetch calls, and how we can hijack or replicate these requests to exploit vulnerabilities.

---

## 🧠 Technical Strategy

The `fetch()` API in JavaScript is used to make asynchronous HTTP requests. It is often seen in Single Page Applications (SPA) like Juice Shop to perform dynamic requests to internal API routes.

When a developer misuses the fetch call or doesn’t properly sanitize the request or protect the API endpoint (e.g., by omitting authorization headers or CSRF protections), it opens up potential for **Fetch Injection** — where an attacker can intercept and replay (or forge) fetch calls to access unauthorized data or perform actions.

In this challenge, we will:
- Discover a vulnerable fetch call.
- Hijack it via browser console or clone it.
- Modify request parameters or endpoint.
- Analyze backend behavior.

---

## 📌 Prerequisites
- Running OWASP Juice Shop locally or on a VM
- Browser with Developer Tools (Chrome preferred)
- Familiarity with JavaScript console and HTTP request structure

---

## 🧪 Step-by-Step Execution

### 🔹 Step 1: Open Developer Tools and Observe JS Fetch Calls
- Open Juice Shop in your browser: `http://localhost:3000`
- Press `F12` or `Ctrl+Shift+I` to open Developer Tools
- Go to the **Network** tab
- Filter by **XHR** (XMLHttpRequest) or **Fetch** to isolate JavaScript network calls

📌 You are looking for calls like:
GET /rest/products/search?q=apple

makefile
Copy code
or:
POST /rest/basket/...

yaml
Copy code

These are internal API routes triggered by `fetch()` from the front-end JavaScript files.

---

### 🔹 Step 2: Identify the Fetch Function via JavaScript Console

Go to the **Console** tab and try to locate global fetch calls:
```js
fetch
Then list all functions calling fetch:

js
Copy code
Object.getOwnPropertyNames(window).filter(prop => {
  try {
    return typeof window[prop] === 'function' && window[prop].toString().includes('fetch');
  } catch (e) { return false; }
})
🔍 Why this matters?
We're checking which JavaScript functions use fetch internally. If they're poorly protected, we can reuse or manipulate them.

🔹 Step 3: Hijack or Replay a Fetch Call
Let’s simulate a new fetch request via the browser console to interact directly with a backend API:

js
Copy code
fetch('/rest/user/login', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    email: "' or 1=1--",
    password: "random"
  })
}).then(res => res.json()).then(console.log)
🔧 What’s happening technically?

We are simulating a login request with an SQL injection payload.

The fetch() method creates a POST request to /rest/user/login.

The response is JSON and will be logged.

🔹 Step 4: Locate Actual JS File Where Fetch Exists
Go to Sources tab → Look for main.*.js

Press Ctrl+F → Search for .fetch(

You’ll find something like:

js
Copy code
window.fetch("/api/admin/dashboard", {...})
📌 Deobfuscate if needed using Prettier or run:

bash
Copy code
js-beautify main.12345.js -r
🔹 Step 5: Modify Fetch Parameters & Inject Payloads
In the console, replicate and modify this call:

js
Copy code
fetch('/api/scoreboard', {
  method: 'GET'
}).then(res => res.json()).then(console.log)
Or send crafted headers:

js
Copy code
fetch('/api/debug', {
  method: 'GET',
  headers: {
    'X-Admin': 'true'
  }
}).then(res => res.text()).then(console.log)
🛡️ Why this works:

Some internal APIs may be accessible only with specific headers.

fetch() allows us to add custom headers.

If endpoint lacks access controls (ACL, auth tokens), we gain unauthorized access.

🔹 Step 6: Observe Network Tab for Backend Behavior
Watch for HTTP Status Codes:

200 OK: success

403 Forbidden: protected

401 Unauthorized: needs credentials

Review the Response Body — contains clues or flags.

🔹 Step 7: Automation (Optional)
Use Node.js to automate:

js
Copy code
const fetch = require('node-fetch');
fetch('http://localhost:3000/rest/user/login', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ email: 'admin@juice-sh.op', password: 'admin123' })
}).then(res => res.json()).then(console.log);
🧠 Why This Method?
Fetch is widely used in front-end JavaScript; it's a common attack surface.

Poorly designed apps may expose internal APIs without CSRF, token auth, or origin verification.

This method mimics real-world API tampering and logic abuse often seen in bug bounties.

💡 Key Concepts Explained
Term	Meaning
fetch()	JavaScript API to make HTTP requests
API Endpoint	Backend URL hit by front-end code
Headers	Metadata sent with HTTP request
Injection	Injecting malicious input into trusted contexts
Auth Bypass	Bypassing authentication by simulating backend calls
XHR	XMLHttpRequest — older async method before fetch
SPA	Single Page Application — dynamic JS-driven frontend

🧾 Final Deliverables
✅ Screenshot of injected fetch call

✅ Capture response with data leak / internal info

✅ Write PoC in console and/or Node.js

✅ Document the endpoint, method, headers, and response

📁 Suggested File Structure
pgsql
Copy code
Method-14_JS-Fetch-Injection/
├── README.md
├── TODO.md
├── screenshots/
│   ├── fetch-response.png
│   └── debug-api-access.png
└── PoC/
    └── injected-fetch.js
pgsql
Copy code
