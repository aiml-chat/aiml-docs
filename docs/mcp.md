# MCP (Model Context Protocol)

AIML.chat speaks [MCP](https://modelcontextprotocol.io) in **both directions**:

- **As a server** — your website's knowledge base is exposed as an MCP tool any AI agent can call.
- **As a client** — you can add an external MCP server as a knowledge source, and its resources are indexed alongside your site.

---

## Your knowledge base as an MCP server

Each website is reachable at a single MCP endpoint over Streamable HTTP:

```
POST https://api.aiml.chat/mcp
X-Api-Key: aiml_pk_your_key
```

- **Auth:** your publishable `aiml_pk_...` key (the same one the widget uses) in the `X-Api-Key` header.
- **Tool exposed:** `search_knowledge_base` — runs a semantic search over that website's indexed content and returns the most relevant chunks with their source URLs.

### Connecting from an MCP client

Most MCP clients accept a remote HTTP server with custom headers. A typical client config looks like:

```json
{
  "mcpServers": {
    "aiml": {
      "url": "https://api.aiml.chat/mcp",
      "headers": { "X-Api-Key": "aiml_pk_your_key" }
    }
  }
}
```

Once connected, the agent can call `search_knowledge_base` with a natural-language query and ground its answers in your content.

---

## Adding an external MCP server as a source

In the dashboard, open your website's **Sources** tab and add a source of type **MCP** (or call the API):

```
POST /v1/websites/{id}/sources
Authorization: Bearer <token>
```

```json
{
  "sourceType": "Mcp",
  "url": "https://your-mcp-server.example.com",
  "authType": "Bearer",
  "authToken": "your-secret",
  "mcpTools": "optional,comma,separated,filter"
}
```

- **Auth types:** `None`, `Bearer`, or `CustomHeader` (set `authHeaderName`).
- **Security:** external MCP servers are fetched **outbound only**. Private/internal addresses (`localhost`, `10.x`, `192.168.x`, link-local metadata) are blocked by default to prevent SSRF. Stored auth tokens are encrypted at rest (AES-GCM).
- **Errors:** if a sync fails, the reason is surfaced as `lastError` on the source so you can fix and re-sync.

---

## Notes

- The MCP server endpoint can be disabled platform-wide via the `Mcp:Enabled` setting (enabled by default).
- The `search_knowledge_base` tool is tenant-scoped: a key only ever sees its own website's content.
