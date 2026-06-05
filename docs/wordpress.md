# WordPress Plugin

The AIML.chat WordPress plugin adds an AI assistant to any WordPress site. It's available from the WordPress plugin directory.

---

## Installation

### From the plugin directory (recommended)

1. In your WordPress admin: **Plugins → Add New**
2. Search for **AIML Chat**
3. Click **Install Now**, then **Activate**

### Manual installation

1. Download `aiml-chat.zip` from your [AIML.chat dashboard](https://aiml.chat/dashboard)
2. Go to **Plugins → Add New → Upload Plugin**
3. Upload the zip and click **Install Now**
4. Activate the plugin

---

## Configuration

Go to **Settings → AIML Chat** in your WordPress admin.

| Setting | Description |
|---------|-------------|
| API Key | Your `aiml_pk_...` key from the dashboard |
| Website ID | Your website ID — **required for lead capture** |
| Enable widget | Toggle the widget on/off site-wide |
| Position | `right` (default) or `left` |
| Theme | `auto`, `light`, or `dark` |
| Brand Colour | Optional hex accent colour (e.g. `#4f46e5`) |
| Exclude pages | URL paths (one per line) where the widget is hidden |

Save changes — the widget will appear on all non-excluded pages.

> Enter your **Website ID** (next to the API key in the dashboard) to enable lead capture — when the assistant can't answer, it invites the visitor to leave their email. Requires plugin **v1.1.0+**.

---

## How it works

The plugin uses `wp_enqueue_scripts` to inject the widget script tag. It passes your API key and website ID as `data-*` attributes:

```html
<!-- Auto-injected by the plugin -->
<script
  src="https://cdn.aiml.chat/v1/widget.js"
  data-api-key="aiml_pk_..."
  data-website-id="..."
  data-position="right"
  data-theme="auto"
  async defer>
</script>
```

---

## Indexing your WordPress content

After installing the plugin:

1. Log in to [aiml.chat/dashboard](https://aiml.chat/dashboard)
2. Your site should already be registered — click **Start indexing**
3. AIML.chat will crawl your posts, pages, and custom post types
4. Re-indexing happens automatically on your configured schedule (daily/weekly)

---

## Excluding pages

Enter URL paths in the **Exclude pages** field, one per line. The widget is hidden on any page whose path matches (or starts with) a listed path:

```
/cart
/checkout
/my-account
```

You can also place the widget on specific pages only with the `[aiml_chat]` shortcode or the **AIML.chat Widget** block instead of the site-wide toggle.

---

## Troubleshooting

**Widget doesn't appear**
- Confirm the plugin is active and the API key is saved
- Check that the current page isn't in the excluded pages list
- Verify your WordPress theme doesn't block external scripts

**"No content indexed yet" error**
- Trigger indexing from your dashboard
- Check that the site URL matches what you registered

**The widget conflicts with my theme**
- The widget uses Shadow DOM — styles should be fully isolated
- If you see layout issues, try switching the `data-position` to `left`

---

## Changelog

See the plugin's [changelog on wp.org](https://wordpress.org/plugins/aiml-chat/#developers) for release notes.
