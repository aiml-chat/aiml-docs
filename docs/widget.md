# Widget Customisation

The AIML.chat widget is a vanilla JS Shadow DOM component (`<30KB` gzipped). It can be customised via `data-*` attributes, CSS custom properties, and the JS API.

---

## Embed snippet

```html
<script
  src="https://cdn.aiml.chat/v1/widget.js"
  data-api-key="aiml_pk_your_key"
  data-website-id="your-website-id"
  data-position="right"
  data-theme="auto"
  async defer>
</script>
```

---

## Script tag attributes

| Attribute | Default | Values |
|-----------|---------|--------|
| `data-api-key` | required | `aiml_pk_...` |
| `data-website-id` | required | UUID |
| `data-position` | `right` | `right`, `left` |
| `data-theme` | `auto` | `light`, `dark`, `auto` |
| `data-primary-color` | — | Any CSS color |
| `data-api-url` | `https://api.aiml.chat` | Override for self-hosted |

---

## Dashboard configuration

The following are configured per-website in the dashboard under **Widget settings**:

| Setting | Description |
|---------|-------------|
| Title | Chat window header (default: "AI Assistant") |
| Subtitle | Below the title (default: "Ask me anything") |
| Placeholder | Input placeholder text |
| Greeting message | First message shown when widget opens |
| Show branding | Toggle "Powered by aiml.chat" badge (paid plans) |

---

## CSS custom properties

The widget respects these CSS custom properties (set them on `:root` or via `data-primary-color`):

| Property | Default (light) | Description |
|----------|----------------|-------------|
| `--aiml-primary` | `#3b82f6` | Accent / send button color |
| `--aiml-bg` | `#ffffff` | Window background |
| `--aiml-surface` | `#f8fafc` | Message bubble background |
| `--aiml-border` | `#e2e8f0` | Border color |
| `--aiml-text` | `#0f172a` | Primary text |
| `--aiml-text-muted` | `#64748b` | Secondary text |
| `--aiml-radius` | `16px` | Window corner radius |
| `--aiml-shadow` | `0 20px 60px ...` | Window drop shadow |

**Example — purple theme:**
```html
<script
  src="https://cdn.aiml.chat/v1/widget.js"
  data-api-key="..."
  data-primary-color="#7c3aed"
  defer>
</script>
```

---

## Dark mode

Set `data-theme="dark"` to force dark mode, or `auto` to follow `prefers-color-scheme`. The widget's dark mode variables:

| Property | Dark value |
|----------|-----------|
| `--aiml-bg` | `#1e293b` |
| `--aiml-surface` | `#0f172a` |
| `--aiml-border` | `#334155` |
| `--aiml-text` | `#f1f5f9` |

---

## JavaScript API

After the widget loads, `window.AIML` is available:

```js
// Open the chat window
window.AIML.open()

// Close the chat window
window.AIML.close()

// Toggle open/closed
window.AIML.toggle()

// Clear conversation history (sessionStorage)
window.AIML.clearHistory()
```

**Example — open on button click:**
```html
<button onclick="window.AIML?.open()">Ask AI</button>
```

---

## Events

The widget fires Custom Events on `window`:

| Event | Detail | Description |
|-------|--------|-------------|
| `aiml:open` | — | Chat window opened |
| `aiml:close` | — | Chat window closed |
| `aiml:message` | `{ role, content }` | Message sent or received |

```js
window.addEventListener('aiml:message', (e) => {
  console.log(e.detail) // { role: 'user', content: '...' }
})
```

---

## Accessibility

The widget is built with accessibility in mind:

- Full keyboard navigation (Tab, Enter, Escape)
- Focus trap while chat is open
- `role="dialog"` on the chat window
- `aria-live="polite"` on the message list
- "AI Assistant" label in header (EU AI Act transparency)
- Session continuity via `sessionStorage` — no cookies
