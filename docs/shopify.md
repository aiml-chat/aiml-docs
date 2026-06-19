# Shopify

Add the AIML.chat AI assistant to your Shopify storefront.

> **The one-click Shopify app is in review and not yet on the App Store.** Until it's listed, add the widget manually (below) — it takes a minute. The app sections that follow describe the experience once it's published.

---

## Manual install (available now)

1. In your [AIML.chat dashboard](https://aiml.chat/dashboard), open the **Integrations** tab and copy your Shopify snippet — your API key and Website ID are already filled in.
2. In your Shopify admin: **Online Store → Themes → Edit code**.
3. Open `theme.liquid` and paste the snippet just before the closing `</body>` tag.
4. **Save.** The assistant now appears on every storefront page.

Appearance (title, greeting, colour, suggested questions) is managed centrally in your dashboard under **Widget settings**.

---

## App install (when published)

1. Install the **AIML.chat** app from the Shopify App Store (or your Partner dev store during testing).
2. On install, the app registers your store with AIML.chat and starts indexing your products, pages, and policies automatically.
3. Open the app from your Shopify admin to see indexing status.

---

## Enable the widget (app)

The widget is delivered as a **Theme App Extension**:

1. In Shopify admin, go to **Online Store → Themes → Customize**.
2. Open **App embeds** (bottom of the left sidebar).
3. Toggle **AIML Chat** on.
4. Configure position, theme, and brand colour, then **Save**.

The widget appears as a floating chat button on all storefront pages.

---

## Settings

Configured in the theme editor under the AIML Chat app embed:

| Setting | Description |
|---------|-------------|
| Enable chat widget | Show/hide the widget |
| API Key | Your `aiml_pk_...` key (auto-filled on install) |
| Website ID | Identifies your store — **required for lead capture** |
| Widget position | Bottom right or bottom left |
| Colour theme | Auto, light, or dark |
| Primary colour | Accent colour to match your brand |

Greeting message, title, and suggested questions are configured in your [AIML.chat dashboard](https://aiml.chat/dashboard).

---

## Re-indexing

Content is re-indexed on a schedule, but after a big catalogue or policy change you can trigger it manually from the app's dashboard with **Re-index store content**.

---

## Privacy & GDPR

The app implements all of Shopify's mandatory GDPR webhooks:

- `customers/data_request` — returns any leads stored for that customer
- `customers/redact` — deletes that customer's leads
- `shop/redact` — deletes all store data after uninstall

The widget itself uses `sessionStorage` (no tracking cookies).

---

## Uninstall

Removing the app marks your install inactive and stops the widget. Store data is deleted following Shopify's `shop/redact` window.
