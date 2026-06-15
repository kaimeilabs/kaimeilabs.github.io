# Kaimei Labs

> **Deterministic verification infrastructure for AI agent outputs.** LLMs generate, we verify. Recipes are the first vertical — the same approach generalises to any procedural domain where correctness matters.

## Guardian Engine

When AI agents generate recipes, they hallucinate — impossible temperatures, skipped techniques, wrong ingredients, broken emulsions. **Guardian Engine catches these errors before they reach the pan.** It verifies each recipe against curated master recipes from professional kitchens using deterministic analysis.

**161 master recipes** across 5 regions — from Confit de Canard to Bulgogi to Jerk Chicken — with new dishes added regularly.

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

### What Does a Verification Report Look Like?

Here's the response structure when Guardian catches authenticity issues in an AI-generated recipe:

```json
{
  "verdict": "FAILED",
  "response_format_version": "v3",
  "findings": [
    {
      "issue": "MISSING_REQUIRED_INGREDIENT",
      "severity": "CRITICAL",
      "justification": "This ingredient provides a signature flavour component essential to the dish's identity."
    },
    {
      "issue": "WRONG_COOKING_MEDIUM",
      "severity": "WARNING",
      "justification": "Cooking medium fundamentally affects texture and flavour."
    }
  ],
  "allergen_warnings": ["milk", "eggs"]
}
```

Each finding includes a `severity` and a `justification` grounded in culinary science — so the agent fixes only what's wrong instead of guessing.

---

### Resources

- **[API Docs & SDK](https://github.com/kaimeilabs/guardian-api-docs)** — Full schema, Python examples, integration tests
- **[Smithery Listing](https://smithery.ai/servers/kaimeilabs/guardian-engine)** — One-click install for Claude & Cursor
- **[Terms of Service](https://kaimeilabs.dev/terms)** — Free early access, fair use, data policy
- **API Endpoint**: `https://api.kaimeilabs.dev/mcp` (Streamable HTTP / MCP)

---

### Why Pass the Prompt?

When you include the user's original request via `original_prompt`, Guardian personalises findings to the user's stated dietary needs and flavour preferences, and activates audience-sensitive safety checks (e.g. flagging honey in recipes for infants, raw egg for pregnant users). Without it, Guardian still returns the full verdict and all findings.

---

*Building an AI cooking assistant, smart kitchen platform, or agentic food-tech product? Contact: partners@kaimeilabs.dev*
