📘 OWASP-Juice-Shop-Confidential-Document-Challenge/Method-08_Sitemap-Discovery/README.md
markdown
Copy code
# 🗺️ Method 08 - Sitemap Discovery

## 📌 Objective
Leverage the presence of `sitemap.xml` or `sitemap.txt` files to discover hidden, sensitive, or undocumented endpoints and files that may expose confidential content in the OWASP Juice Shop application.

---

## 🧠 Concept
Search engines use `sitemap.xml` to index site content. Misconfigured or overly permissive sitemaps may unintentionally expose non-public resources like:
- Forgotten admin panels
- Downloadable backups
- Hidden flags or documentation
- Disallowed paths bypassing `robots.txt`

---

## 🛠️ Tools Required
- `curl` or `wget`
- `xmllint` (optional for pretty parsing)
- Browser (optional)

---

## 🧪 Methodology

### 🔍 Step 1: Try Default Sitemap Paths
```bash
curl -s http://<target>/sitemap.xml
curl -s http://<target>/sitemap.txt
🔍 Step 2: Analyze Sitemap Content
Look for URLs pointing to:

/ftp, /backup, /docs

Suspicious file types: .pdf, .bak, .zip, .conf

🔍 Step 3: Crawl or Manually Visit Suspicious URLs
Visit each link manually or automate the download

bash
Copy code
wget -r -np -nH --cut-dirs=1 -R "index.html*" -i sitemap.txt
🎯 Goal
Locate the confidential document via sitemap-exposed paths.

Validate and exploit the access to non-indexed or sensitive endpoints.

✅ Indicators of Success
Successfully downloading a confidential file (e.g., PDF).

Finding exposed admin/debug functionality.

Identifying paths not linked in the UI but referenced in sitemap.xml.

⚠️ Notes
Many real-world apps forget to restrict sensitive URLs in sitemap.xml.

Combine with robots.txt analysis for better endpoint coverage.

javascript
Copy code
