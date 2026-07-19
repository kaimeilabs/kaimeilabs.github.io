# Kaimei Labs

> **Deterministic verification infrastructure for AI agent outputs.** Your LLM authors, Guardian judges. Recipes are the first vertical — the same approach generalises to any procedural domain where correctness matters.

## Guardian Engine

When AI agents generate recipes, they hallucinate — impossible temperatures, skipped techniques, wrong ingredients, broken emulsions. **Guardian Engine is a deterministic oracle inside your agent's generate→verify loop**: it catches these errors before they reach the pan and returns machine-actionable patches so the agent can fix exactly what's wrong.

An LLM critique is a *sample* — it misses differently on every run. Guardian's symbolic engine makes guarantees a generative model structurally cannot:

- **Exhaustive** — every rule is checked on every call, not a sample of them.
- **Certified negatives** — "no EU Annex II allergen source detected" is an absence claim an LLM cannot make.
- **Replayable** — same input + same spec + same knowledge-base version → byte-identical verdict, pinned by `kb_version_hash` and `master_hash` for audit.
- **Machine-actionable repair** — structured patches, plus a deterministic `fix_recipe` tool (no LLM in the loop).
- **Bring your own spec** — verify against *your* house recipe or SOP via `master_json`, not just our catalog.

**161 master recipes** across 5 regions — from Confit de Canard to Bulgogi to Jerk Chicken — with new dishes added regularly. Seven MCP tools: `verify_recipe`, `fix_recipe`, `list_dishes`, `get_master`, `check_safety`, `check_allergens`, and `verify_dietary_claim` (vegan / vegetarian / gluten-free / dairy-free / nut-free / halal / kosher).

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

Here's the (abridged) response structure when Guardian catches issues in an AI-generated recipe:

```json
{
  "response_format_version": "v3",
  "kb_version_hash": "7dda40a3b646",
  "verdict": "FAILED",
  "matched_against": "Pasta alla Carbonara (Master)",
  "master_source": "catalog",
  "master_hash": "3357e09...41fd29",
  "summary": {"CRITICAL": 4, "WARNING": 0, "INFO": 3},
  "findings": [
    {
      "issue": "TEMPERATURE_MISMATCH",
      "severity": "critical",
      "justification": "Temperature is significantly outside the required range.",
      "details": {"expected": "100.0-130.0", "observed": "180"}
    },
    {
      "issue": "INGREDIENT_SUBSTITUTED",
      "severity": "critical",
      "justification": "'bacon' is in the same group ('cured_pork') as 'guanciale' but is not the canonical ingredient for this recipe.",
      "details": {"expected": "guanciale", "observed": "bacon"}
    }
  ],
  "allergens": [
    "Allergen detected: Milk and products thereof (including lactose)",
    "Allergen detected: Cereals containing gluten (wheat, rye, barley, oats, spelt, kamut)"
  ],
  "patches": [
    {"action": "set_temperature", "step_index": 2, "value": "100.0-130.0"},
    {"action": "replace_ingredient", "remove": "bacon", "add": "guanciale"}
  ]
}
```

Each finding includes a `severity`, a `justification` grounded in culinary science, and a machine-actionable patch — so the agent fixes only what's wrong instead of guessing. The `kb_version_hash` and `master_hash` pin the exact knowledge-base and spec versions, making every verdict replayable as an audit record.

> ⚠️ **Safety note:** Guardian output — including allergen warnings and dietary-claim checks — is automated and informational only. It is not food-safety, medical, or certification advice, and a `PASSED` verdict is never a guarantee that a food is allergen-free or safe for any individual. See the [Terms of Service](https://kaimeilabs.dev/terms).

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
