📁 OWASP-Juice-Shop-Confidential-Document-Challenge/Method-08_Sitemap-Discovery/TODO.md
markdown
Copy code
# ✅ TODO - Sitemap Discovery Method

## [ ] Step 1: Identify Sitemap Files
- [ ] Use `curl` to request common sitemap paths:
  - `http://<target>/sitemap.xml`
  - `http://<target>/sitemap.txt`

## [ ] Step 2: Parse Sitemap Content
- [ ] Look for hidden or uncommon URLs
- [ ] Note URLs pointing to potentially sensitive or confidential files

## [ ] Step 3: Explore Discovered Endpoints
- [ ] Access the files in browser or download using `wget`
- [ ] Inspect if any lead to:
  - PDF reports
  - Internal documentation
  - Hidden admin pages

## [ ] Step 4: Validate the Confidential Document
- [ ] Confirm file relevance to the challenge
- [ ] Take screenshots of the document or response body as PoC

## [ ] Step 5: Archive Findings
- [ ] Save the downloaded file locally
- [ ] Document HTTP request & response in markdown or screenshot
