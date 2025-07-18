🛠️ Method 12: Admin Debug Path Leak – OWASP Juice Shop Challenge
🎯 Objective
Exploit misconfigured or verbose error/debug output that leaks internal admin or debug-only paths to access sensitive endpoints or features not meant for public users.

🧵 Background – Why This Works
Modern web applications often expose debug information or internal admin endpoints during development. When these endpoints or logs are accidentally left exposed in production, they present a severe information disclosure vulnerability.

Key concepts involved:

Verbose Error Messaging: Improper error handling that returns stack traces or debug logs.

Hidden Administrative Interfaces: Endpoints like /debug, /admin, /rest/debug, etc., used for internal testing.

Security Misconfiguration (OWASP A05): When security headers or access control around debug tools are not disabled before production.

⚙️ Technical Strategy
Our strategy is to:

Trigger an error (e.g., by navigating to a broken or non-existent path).

Review the verbose error/debug page for hints such as:

Internal file structures

Path leaks (/admin, /debug, /internal)

Developer comments

Navigate to those endpoints manually or use automated enumeration.

🔎 Methodology – Step-by-Step Guide
🔸 Step 1: Start the Application
bash
Copy code
docker run -d -p 3000:3000 --name juice-shop bkimminich/juice-shop
🛠️ Starts Juice Shop on port 3000.

🔸 Step 2: Open Juice Shop
Visit: http://localhost:3000

🔸 Step 3: Trigger a Debug/Error Response
In the browser, manually enter a non-existent route, such as:

bash
Copy code
http://localhost:3000/doesnotexist
This should trigger a 404 Not Found or 500 Internal Server Error.

🔬 What Happens Technically?
Juice Shop's Angular front-end or Node.js back-end catches the missing route.

Depending on environment config (NODE_ENV=development), debug traces may be shown.

In poorly configured setups, this reveals internal route logic and sometimes unprotected endpoints.

🔸 Step 4: Analyze the Response
Check for:

Path disclosures like:

bash
Copy code
at /app/routes/admin/debug.js
at /app/api/internal/logs.js
Backend file structures (stack traces often reveal internal file locations).

Developer comments in HTML or response headers.

🔸 Step 5: Access Leaked Paths
If you find /debug, /admin, or /internal/logs:

Visit them in the browser:

bash
Copy code
http://localhost:3000/debug
http://localhost:3000/rest/admin
Use curl to probe:

bash
Copy code
curl -I http://localhost:3000/rest/admin
🔸 Step 6: Capture Sensitive Info
If any endpoint exposes:

User logs

Stack traces

Credentials or tokens

Deployment info (e.g., debug=true, dev_mode=1)

🎯 You have successfully exploited debug path leakage.

🔐 Security Lesson
Always set NODE_ENV=production.

Disable verbose logging in client/server.

Restrict admin/debug endpoints via proper authentication and RBAC.

Scan production releases using tools like:

Nikto, dirb, ffuf for directory fuzzing.

Burp Suite to capture verbose responses.

📦 Output Example
http
Copy code
500 Internal Server Error
...
ReferenceError: debugLogger is not defined
    at /app/routes/admin/debug.js:47:15
Leak: Internal file path /app/routes/admin/debug.js

