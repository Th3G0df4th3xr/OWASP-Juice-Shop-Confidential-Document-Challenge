📁 Method-10_Error-Page-Hint/README.md
markdown
Copy code
# 🧩 Method 10: Error Page Enumeration for Sensitive Route Discovery

## 🔍 Overview
Web applications often leak sensitive information through error pages such as `403 Forbidden`, `404 Not Found`, `500 Internal Server Error`, or custom access-denied messages. By analyzing these errors, attackers can infer the presence of protected or misconfigured routes.

This technique focuses on fuzzing and pattern-based brute-forcing to trigger informative error responses. Subtle variations in error messages, response lengths, or headers often act as breadcrumbs leading to hidden resources.

## 🎯 Objective
Leverage differential error response behavior to uncover:
- Hidden admin portals
- Forbidden API routes
- Debug endpoints
- Misconfigured access control logic

## 🔧 Tools
- `ffuf`, `wfuzz`, or `burp intruder`
- Custom wordlists (common admin paths, API routes, framework defaults)
- `curl`/`httpx` for manual probing
- `diff`, `wc -c`, `grep` for response comparison

## 🧪 Strategy
1. Fuzz for common paths and observe how error responses differ.
2. Monitor content length, status codes, redirect behavior.
3. Filter responses showing anomalies (e.g., 403 vs 404 vs 200).
4. Pivot from pages showing signs of internal references or partial access.

## 📌 Example Error Patterns
- `403 - Access Denied (but custom error page reveals route)`
- `404 - Page Not Found (with stack trace or framework leak)`
- `401 - Requires authentication (possible session-controlled endpoint)`

## ⚠️ Caution
Always correlate with app behavior. Error-based discovery may be noisy or rate-limited.

## 🔓 Outcome
Gain visibility into sensitive or hidden routes by intelligently analyzing error handling behavior — a classic technique for uncovering what lies just beneath the surface.
