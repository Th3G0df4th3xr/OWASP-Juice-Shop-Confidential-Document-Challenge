📄 OWASP-Juice-Shop-Confidential-Document-Challenge/Method-03_DevTools-Network-Inspection/TODO.md
markdown
Copy code
# ✅ TODO - Method 03: DevTools Network Inspection

## [ ] 1. Start the OWASP Juice Shop container
- [ ] Run: `docker run -d -p 3000:3000 --name juice-shop bkimminich/juice-shop`
- [ ] Confirm Juice Shop is accessible at `http://localhost:3000`

## [ ] 2. Open Browser DevTools
- [ ] Launch Chrome or Firefox
- [ ] Open DevTools (`F12` or right-click → Inspect)
- [ ] Navigate to the **Network** tab

## [ ] 3. Reload the Page to Populate Network Logs
- [ ] Press `Ctrl + R` to refresh the page
- [ ] Observe all HTTP requests in the **Network** panel

## [ ] 4. Identify Suspicious or Sensitive Resources
- [ ] Look for:
  - File types like `.md`, `.bak`, `.zip`, `.conf`, `.log`, `.backup`
  - URLs with `/ftp/`, `/support/`, `/files/`, `/confidential/`
  - Response status: `200 OK`

## [ ] 5. Inspect JavaScript Files
- [ ] Go to the **Sources** tab
- [ ] Review scripts for:
  - Hidden endpoints
  - Embedded URLs
  - Clues for sensitive file paths

## [ ] 6. Manually Visit Identified URLs
- [ ] Copy the URL from Network or JS file
- [ ] Paste into browser to validate the exposure
- [ ] Example: `http://localhost:3000/ftp/acquisitions.md`

## [ ] 7. Document Your Findings
- [ ] Record exposed file paths
- [ ] Note the HTTP method and response code
- [ ] Save screenshots or terminal output (optional)

## [ ] 8. Analyze & Mitigate (for blue team perspective)
- [ ] List security issues identified
- [ ] Recommend proper access control & sanitization

