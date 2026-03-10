# Kaimei Labs

> **Verification infrastructure for AI-generated content.** LLMs generate, we verify.

## Guardian Engine

When AI agents generate recipes, they hallucinate — impossible temperatures, skipped techniques, wrong ingredients, broken emulsions. **Guardian Engine catches these errors before they reach the pan.** It verifies each recipe against curated master recipes from professional kitchens using deterministic graph-based analysis.

**20 master recipes** across 12 global cuisines — from Confit de Canard to Thai Green Curry — with new dishes added regularly.

[![Install with Smithery](https://smithery.ai/install-badge.svg)](https://smithery.ai/servers/kaimeilabs/guardian-engine)

---

### Connect in 30 Seconds

Guardian is a hosted MCP server. No install, no API key, no Docker.

**Claude Desktop** — add to `claude_desktop_config.json`:
```json
{
  "mcpServers": {
    "guardian": {
      "url": "https://api.kaimeilabs.dev/mcp",
      "transport": "streamable-http"
    }
  }
}
```

**Cursor** — add via Settings → MCP Servers:
```json
{
  "guardian": {
    "url": "https://api.kaimeilabs.dev/mcp",
    "transport": "streamable-http"
  }
}
```

**VS Code** — add to `.vscode/mcp.json`:
```json
{
  "servers": {
    "guardian": {
      "type": "http",
      "url": "https://api.kaimeilabs.dev/mcp"
    }
  }
}
```

**Python SDK** — `pip install mcp httpx`:
```python
from mcp.client.session import ClientSession
from mcp.client.streamable_http import streamable_http_client
result = await session.call_tool("verify_recipe", arguments={"dish": "carbonara", "candidate_json": recipe_json})
```

---

### Resources

- **[API Docs & SDK](https://github.com/kaimeilabs/guardian-api-docs)** — Full schema, Python examples, integration tests
- **[Smithery Listing](https://smithery.ai/servers/kaimeilabs/guardian-engine)** — One-click install for Claude & Cursor
- **[Terms of Service](https://kaimeilabs.dev/terms)** — Free early access, fair use, data policy
- **API Endpoint**: `https://api.kaimeilabs.dev/mcp` (Streamable HTTP / MCP)

---

### Why Pass the Prompt?

When you include the user's original request via `original_prompt`, Guardian activates **Guided Oracle Mode** — returning specific, actionable improvement tips tailored to what the user actually asked for (dietary needs, flavor profiles, technique choices). Without it, you get a basic Pass/Fail verdict.

---

*Contact: partners@kaimeilabs.dev*
