# Getting Started

Get an AI chat assistant answering questions from your website in under 5 minutes.

---

## 1. Create an account

Go to [aiml.chat/register](https://aiml.chat/register) and sign up with your email — or use **Continue with Google** for one-click sign-in.

---

## 2. Add your website

In the dashboard, click **Add website** and enter your domain (e.g. `docs.example.com`).

You'll receive:
- A **Website ID** — identifies your site in API calls
- An **API Key** — authenticates widget requests (prefix: `aiml_pk_`)

Keep the API key safe. It can be rotated from the dashboard at any time.

---

## 3. Index your content

Click **Start indexing** (or call `POST /v1/websites/{id}/ingest`). AIML.chat will:

1. Fetch your sitemap (or crawl from your root URL)
2. Extract text from each page (strips nav, footer, ads)
3. Split text into 512-token chunks with 64-token overlap
4. Generate vector embeddings and store them

Indexing runs in the background. Watch progress on the website status page.

Beyond crawling your domain, you can add more **knowledge sources** — a public GitHub repo, an uploaded PDF / Word doc, pasted text, or an external MCP server — from the website's **Sources** tab. See the [API reference](./api-reference.md#knowledge-sources).

```bash
# Trigger indexing via API
curl -X POST https://api.aiml.chat/v1/websites/{id}/ingest \
  -H "Authorization: Bearer YOUR_JWT" \
  -H "Content-Type: application/json" \
  -d '{"startUrl": "https://docs.example.com"}'
```

---

## 4. Embed the widget

Add one script tag before `</body>` on your site:

```html
<script
  src="https://cdn.aiml.chat/v1/widget.js"
  data-api-key="aiml_pk_your_key_here"
  data-website-id="your-website-id"
  async defer>
</script>
```

The widget loads asynchronously — it never blocks your page render.

> **Tip:** the dashboard **Integrations** tab has the snippet above with your key and Website ID already filled in, a one-click **WordPress plugin** download, and **Shopify** instructions — so you can copy/install without hand-editing anything.

---

## 5. Test it — in the dashboard or on your site

You can chat with your assistant **before you deploy anything**: open your website's **Widget** tab and use the live preview — it's the real widget, with your suggested questions, FAQ and citations.

Then verify the embed: open your site in a browser, click the chat bubble, and ask a question about your content. You should see a streamed response with source citations.

> If the widget loads but answers fail with *"not authorized for this domain"*, your key is bound to your registered domain. Add the domain you're testing on under **Widget settings → Allowed domains**. See [Widget customisation](./widget.md#allowed-domains-key-security).

---

## Configuration options

All options are set as `data-*` attributes on the script tag:

| Attribute | Default | Description |
|-----------|---------|-------------|
| `data-api-key` | required | Your API key |
| `data-website-id` | required | Your website ID |
| `data-position` | `right` | `right` or `left` |
| `data-theme` | `auto` | `light`, `dark`, or `auto` |
| `data-primary-color` | — | CSS color (e.g. `#3b82f6`) |

Additional configuration (title, placeholder, greeting message) is set in the dashboard under **Widget settings**.

---

## Next steps

- [API Reference](./api-reference.md) — all endpoints with curl examples
- [Widget customisation](./widget.md) — CSS variables, events, JS API
- [WordPress plugin](./wordpress.md) — install and configure
