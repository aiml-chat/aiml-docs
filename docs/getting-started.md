# Getting Started

Get an AI chat assistant answering questions from your website in under 5 minutes.

---

## 1. Create an account

Go to [aiml.chat/register](https://aiml.chat/register) and sign up with your email.

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

---

## 5. Verify it works

Open your site in a browser and click the chat bubble. Ask a question about your content. You should see a streamed response with source citations.

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
