# FAQ & Troubleshooting

---

## General

**What is AIML.chat?**  
AIML.chat is an AI documentation and website assistant platform. You add a single script tag to your site and visitors get an AI chat widget that answers questions grounded in your own content — with cited sources.

**Does it hallucinate?**  
We instruct the AI to only answer based on the indexed content and to say "I don't know" when the answer isn't in the docs. When no relevant content is found, visitors see a lead capture form instead of a made-up answer.

**What AI model does it use?**  
AIML.chat uses `gpt-4.1-mini` for answers and `text-embedding-3-small` for vector search. Model selection is designed for cost efficiency at scale.

**Is the widget open source?**  
Yes. The [aiml-widget](https://github.com/aiml-chat/aiml-widget) repository (MIT) contains the full vanilla JS widget source. Contributions welcome.

---

## Indexing

**How long does indexing take?**  
Small sites (< 100 pages) typically index in under 2 minutes. Larger sites can take 10–30 minutes depending on content volume and server response times.

**What content types are supported?**  
- HTML pages (crawled via URL)
- PDF and Word (`.docx`) files (uploaded)
- Markdown / plain text (direct upload, pasted text, or GitHub repo URL)
- Sitemaps (auto-detected at `/sitemap.xml`)
- External MCP servers (their resources are indexed as a source)

**Do you support MCP (Model Context Protocol)?**  
Both directions. Your knowledge base is exposed as an MCP server at `https://api.aiml.chat/mcp` (authenticate with your `aiml_pk_` key) with a `search_knowledge_base` tool, and you can add an external MCP server as a knowledge source. See the [MCP guide](./mcp.md).

**Does it respect robots.txt?**  
Yes. AIML.chat checks `robots.txt` before crawling any URL and skips disallowed paths.

**What if a page changes?**  
Re-indexing happens on your configured schedule (daily or weekly). Only changed pages are re-embedded — unchanged pages (same hash) are skipped to save cost.

**How do I index a GitHub repository?**  
Use `POST /v1/websites/{id}/ingest/github` with the repo URL. AIML.chat extracts all `.md` and `.rst` files from the default branch.

---

## Widget

**Does the widget block my page render?**  
No. The script tag includes `async defer` — the widget never blocks page load. It's also only 7KB gzipped.

**Can I remove the "Powered by aiml.chat" badge?**  
Yes — Pro plan and above can toggle the badge off in **Widget settings**.

**Is dark mode supported?**  
Yes. Set `data-theme="dark"` to force dark mode, or `auto` to follow `prefers-color-scheme`.

**Does it work without JavaScript?**  
No — the widget is a client-side component. Users with JS disabled won't see the widget.

**Is there a React/Vue component?**  
Not yet — the widget is vanilla JS with Shadow DOM for maximum compatibility. A React wrapper is on the roadmap.

**Is there a Shopify app?**  
Yes. The Shopify app installs the widget via a Theme App Extension and auto-indexes your products, pages, and policies. See the [Shopify guide](./shopify.md).

---

## Billing

**Is there a free plan?**  
Yes. The free plan includes 100 messages/month, 1 website, and 50 indexed pages. No credit card required.

**Is there a plan for open-source projects?**  
Yes — the OSS Free plan includes 500 messages/month and 200 indexed pages. Apply from your dashboard.

**Who handles taxes?**  
Paddle is the Merchant of Record — they handle VAT, GST, and US sales tax globally. You don't register for foreign taxes.

**Can I cancel anytime?**  
Yes. Cancel from the customer portal and your plan reverts to Free at the end of the billing period.

---

## Privacy & GDPR

**Do you use cookies?**  
The widget uses `sessionStorage` for conversation continuity — not cookies. No cookie banner is required.

**Can users delete their data?**  
Site owners can delete individual conversations via the dashboard or `DELETE /v1/conversations/{id}`. Full account deletion (with 30-day grace period) is available at `DELETE /v1/account`.

**Where is data stored?**  
Data is stored in PostgreSQL hosted on Railway (EU region for EU customers on request).

**Do you train on my content?**  
No. Your indexed content is used only to answer questions on your site. It's not used to train any AI models.

---

## Troubleshooting

**The widget shows "No content indexed yet"**  
Trigger indexing from the dashboard. The widget will show "I don't know" style answers until at least some pages are indexed.

**Answers cite the wrong page**  
Re-index after making significant content changes. If certain pages should be prioritised, check that they're not accidentally blocked by robots.txt.

**The widget doesn't appear on my WordPress site**  
1. Confirm the plugin is active
2. Check the API key is saved in Settings → AIML Chat
3. Confirm the page isn't in the excluded pages list
4. Look for JS errors in the browser console

**Rate limit errors (429)**  
The free plan has a rate limit of 10 requests/hour per IP. Upgrade to Pro or above for higher limits. The `Retry-After` header tells you how many seconds to wait.
