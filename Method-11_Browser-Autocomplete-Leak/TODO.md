✅ TODO - Method-11_Browser-Autocomplete-Leak
🧪 Objective:
Detect and exploit insecure browser autocomplete behavior to uncover hidden administrative or sensitive pages.

🛠️ Step-by-Step Tasks:
 Step 01 – Launch Juice Shop in Browser (Kali)

Open your Juice Shop instance at http://localhost:3000 in Firefox or Chromium.

 Step 02 – Open Developer Tools

Press Ctrl+Shift+I or right-click → Inspect.

Go to the Elements or Inspector tab.

 Step 03 – Visit Login/Register Page

Navigate to http://localhost:3000/#/login or /register.

Locate the <input> elements such as email, password, etc.

 Step 04 – Analyze Input Fields for autocomplete="off"

If fields have autocomplete="off", manually remove that attribute in DevTools.

⚙️ Technical Reason: Modern browsers won’t offer saved credentials if autocomplete=off is present. Removing it simulates older or misconfigured websites.

 Step 05 – Check for Autocomplete Suggestions

Click inside the form fields.

Check if the browser autofills previously saved credentials.

Look for suggestions like:

/admin

/score-board

/rest/admin

 Step 06 – Cross-Verify Autofilled Path

Use autofilled values to probe endpoints using:

bash
Copy code
curl -I http://localhost:3000/admin
curl -I http://localhost:3000/score-board
 Step 07 – Automate with Burp or Custom Script

Intercept form requests using Burp Suite.

Look for headers and values populated via browser autofill.

Optional: use Selenium script to automate autofill behavior across endpoints.

📌 Final Goal:
If the browser exposes sensitive paths via autocomplete (like /admin), it signifies a client-side information disclosure vulnerability.

