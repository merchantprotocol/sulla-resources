# Discover and Call Integrations

All integrations, MCP servers, and internal tools are callable through the `sulla` CLI. The system handles authentication — credentials are resolved from the encrypted vault and injected automatically.

## Three Call Patterns

```bash
# Proxy call — authenticated API pass-through to any service
sulla <account_id>/<slug> '{"method":"GET","path":"/rest/companies"}'

# Internal tool — built-in tools organized by category
sulla <category>/<tool> '{"param":"value"}'

# MCP tool — Model Context Protocol servers
sulla <account_id>/mcp/<tool> '{"param":"value"}'
```

## Discovering What's Available

### List accounts for an integration
```bash
sulla vault/list_accounts '{"account_type":"twenty"}'
sulla vault/list_accounts '{"account_type":"slack"}'
sulla vault/list_accounts '{"account_type":"youtube"}'
sulla vault/list_accounts '{"account_type":"mcp"}'
```

The response shows all accounts with their `account_id`. The one marked `★ ACTIVE` is the default.

### Check if an integration is connected
```bash
sulla vault/is_enabled '{"account_type":"github"}'
```

### See what credentials are stored
```bash
sulla vault/read_secrets '{"account_type":"twenty"}'
```

### List all vault website credentials
```bash
sulla vault/list '{}'
```

## Proxy Calls (API Pass-Through)

Every configured integration uses the same proxy pattern. You provide the HTTP method, path, and body. The system resolves the base URL and injects credentials.

```bash
sulla <account_id>/<slug> '{"method":"<METHOD>","path":"<API_PATH>","body":{...},"query":{...}}'
```

### How Auth Resolution Works

The proxy resolves credentials in this order:
1. **bearer_token** — injected as `Authorization: Bearer <token>` header
2. **OAuth access token** — if OAuth is configured, auto-refreshes and injects
3. **api_key** — injected as `?key=<value>` query parameter

If the caller passes an `Authorization` header in the `headers` field, vault auth is skipped — the caller's header is used as-is.

### Proxy Response Format

```json
{
  "success": true,
  "status": 200,
  "result": { ... }    // Raw upstream response body
}
```

The proxy returns the exact upstream status code. A `200` from the upstream means `success: true`. A `401` or `500` from the upstream means `success: false` with the error in `result`.

### Examples

**REST API calls:**
```bash
# GET
sulla local_merchant_protocol/twenty '{"method":"GET","path":"/rest/companies"}'

# POST with body
sulla local_merchant_protocol/twenty '{"method":"POST","path":"/rest/companies","body":{"name":"Acme Corp"}}'

# PATCH
sulla local_merchant_protocol/twenty '{"method":"PATCH","path":"/rest/companies/UUID","body":{"employees":500}}'

# DELETE
sulla local_merchant_protocol/twenty '{"method":"DELETE","path":"/rest/companies/UUID"}'

# GET with query params
sulla local_merchant_protocol/twenty '{"method":"GET","path":"/rest/companies","query":{"limit":"10","order_by":"name"}}'
```

**GraphQL:**
```bash
sulla local_merchant_protocol/twenty '{"method":"POST","path":"/graphql","body":{"query":"{ companies { edges { node { id name } } } }"}}'
```

## Internal Tools

Built-in tools organized by category. These don't go through the proxy — they execute directly in the Sulla Desktop process.

```bash
sulla <category>/<tool_name> '{"param":"value"}'
```

### Commonly Used

```bash
# Slack
sulla slack/send_message '{"channel":"C01234","text":"Hello"}'
sulla slack/search_users '{"query":"jonathon"}'

# Git/GitHub
sulla github/git_status '{}'
sulla github/git_commit '{"message":"fix: resolve bug"}'
sulla github/create_issue '{"owner":"org","repo":"project","title":"Bug"}'

# Docker
sulla docker/ps '{}'
sulla docker/logs '{"container":"twenty-crm-server","tail":"50"}'

# Vault
sulla vault/list_accounts '{"account_type":"slack"}'
sulla vault/set_credential '{"account_type":"twenty","property":"bearer_token","value":"sk-xxx"}'

# Browser
sulla playwright/browser_tab '{"action":"upsert","url":"https://example.com"}'
sulla playwright/get_page_text '{"assetId":"browser_123"}'

# Database
sulla pg/query '{"query":"SELECT * FROM users LIMIT 10"}'
sulla redis/get '{"key":"mykey"}'

# Notifications
sulla chrome/notify_user '{"title":"Done","message":"Build complete"}'

# Memory
sulla meta/add_observational_memory '{"priority":"🟡","content":"User prefers dark mode"}'

# File operations
sulla meta/exec '{"command":"cat /etc/hosts"}'
sulla meta/file_search '{"query":"authentication middleware"}'
```

## MCP Server Tools

MCP tools use a three-part path: `<account_id>/mcp/<tool_name>`

```bash
sulla wordpress/mcp/list_posts '{}'
sulla wordpress/mcp/create_post '{"title":"New Post","content":"Hello world"}'
sulla wordpress/mcp/ping '{}'
sulla dataripple/mcp/ping '{}'
```

### MCP Response Format

```json
{
  "success": true,
  "result": {
    "success": true,
    "content": [{"type": "text", "text": "{...}"}],
    "isError": false
  }
}
```

MCP tool output is in the `content` array. Most tools return `type: "text"` with a JSON string in `text` — parse it with `json.loads()` if needed.

### Find MCP accounts
```bash
sulla vault/list_accounts '{"account_type":"mcp"}'
```

## Storing Credentials

When you need to save a new API key, token, or base URL:

```bash
sulla vault/set_credential '{"account_type":"twenty","property":"bearer_token","value":"eyJ..."}'
sulla vault/set_credential '{"account_type":"youtube","property":"base_url","value":"https://www.googleapis.com/youtube/v3"}'
```

Common properties: `bearer_token`, `api_key`, `base_url`, `access_token`, `password`

## Integration Documentation

- **Per-integration docs:** `~/sulla/integrations/<slug>/INTEGRATION.md`
- **Tool reference:** `~/sulla/integrations/tools/`
- **Browser tools:** `~/sulla/integrations/tools/browser/`
- **Environment docs:** `~/sulla/integrations/environment/`
- **Auth configs:** `~/sulla/integrations/<slug>/<slug>.v*-auth.yaml`

## Troubleshooting

| Error | Cause | Fix |
|-------|-------|-----|
| `No base_url configured` | Integration missing base URL | `sulla vault/set_credential '{"account_type":"...","property":"base_url","value":"https://..."}'` |
| `WORKSPACE_NOT_FOUND` | Bearer token not scoped to workspace | Regenerate API key from service UI |
| `UNAUTHENTICATED` / 401 | Token expired or invalid | Update with `vault/set_credential` |
| `fetch failed` | Target service not running | Check with `sulla docker/ps '{}'` |
| `Cannot connect` | sulla-daemon not running | Run `sulla-daemon start` or use direct TCP fallback |
| `Tool not found` | Wrong category or name | Check `~/sulla/integrations/tools/` for correct names |
| Account ID unknown | Need to discover accounts | `sulla vault/list_accounts '{"account_type":"..."}'` |

## Tips

- Always check `vault/list_accounts` before guessing account IDs
- Proxy calls return the raw upstream response — debug from the actual error
- If you get auth errors, check credentials with `vault/read_secrets`
- Use `vault/set_credential` to store new API keys or tokens
- Read `~/sulla/integrations/<slug>/INTEGRATION.md` for service-specific details
- For services with GraphQL, use introspection to discover the schema
