📁 OWASP-Juice-Shop-Confidential-Document-Challenge/Method-06_Scoreboard-Leak/TODO.md
markdown
Copy code
# ✅ TODO - Method 06: Scoreboard Leakage Technique

## 🎯 Objective
Leverage Juice Shop's internal scoreboard to enumerate and identify hidden/unlisted challenges that may indirectly expose confidential documents or file access vulnerabilities.

## 🛠️ Action Items

- [ ] Navigate to the scoreboard:
  - Manually access it at `http://<TARGET_IP>:3000/score-board`
  - Or, if hidden: try URL fuzzing to enumerate it.
  
- [ ] Enable browser DevTools (`F12`) to inspect:
  - JavaScript logic behind the scoreboard
  - Background API calls or endpoints (like `/rest/continue-code` or hidden flags)

- [ ] Look for undocumented challenges:
  - Check for names like "Confidential Document", "Sensitive Files", etc.
  - Cross-reference the challenge IDs with their internal identifiers in browser memory (e.g., using console commands like `angular.element(document.body).injector().get('ChallengeService').challenges`)
  
- [ ] Use the challenge hints:
  - Identify file paths or object names that correlate with other challenge types (like disclosure or access controls)

- [ ] Trace scoreboard updates:
  - Use browser console to capture WebSocket or AJAX calls
  - Identify the exact backend responses using the “Network” tab in DevTools

- [ ] Document all challenge metadata:
  - ID, category, difficulty, solved/unresolved status, related backend resource (API or file)

- [ ] Correlate scoreboard metadata with file locations manually discovered earlier via `FFUF`, `dirsearch`, etc.

## 📓 Notes
- Consider combining this with methods like `Source-Code-Review`, `DevTools-Inspection`, and `FFUF` enumeration.
- This is a semi-passive information gathering method that leverages unprotected frontend logic.
