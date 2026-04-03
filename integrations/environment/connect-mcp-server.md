---
schemaversion: 1
slug: connect-mcp-server
title: "Connect an MCP Server as an Integration"
section: "Standard Operating Procedures"
category: "Developer Tools"
tags:
  - skill
  - mcp
  - integration
  - model-context-protocol
  - tools
  - discovery
  - configapi
order: 6
locked: true
author: seed
---

# Connect an MCP Server as an Integration

**Triggers**: Human says "connect MCP", "add MCP server", "register MCP", "set up MCP", "MCP integration", "new MCP", "connect to MCP", "MCP tools".

Register an MCP (Model Context Protocol) server, discover its tools, and generate integration configuration files so the tools become available through the standard integration system — callable by any agent, workflow, or skill.

---

## Why This Matters

MCP servers expose tools via a JSON-RPC protocol, not REST. The integration system is built for REST APIs. This skill bridges the gap: it registers the MCP server, discovers all its tools, and auto-generates YAML integration configs with `transport: mcp` so they route through the MCP protocol while appearing as standard integration endpoints.

After finalization, MCP tools show up in `/v1/tools/list` alongside Slack, GitHub, and every other integration — fully discoverable, with typed parameters, and callable through the same unified pattern.

---

## Prerequisites

You need:
1. The MCP server's **URL** (e.g., `http://localhost:3001/mcp`, `https://mcp.example.com/sse`)
2. An optional **auth token** (bearer token) if the server requires authentication
3. The server must be running and reachable from the Sulla desktop host

---

## Full Workflow (4 Steps)

### Step 1: Register the MCP Account

Save the server URL and auth token to the integration service. Choose an `account_id` that's a short, descriptive slug (lowercase, hyphens ok).

```bash
sulla/v1/mcp/register \
  -H "Content-Type: application/json" \
  -d '{
    "account_id": "my-mcp-server",
    "server_url": "http://localhost:3001/mcp",
    "auth_token": "optional-bearer-token",
    "label": "My MCP Server"
  }' | python3 -c "import sys, json; print(json.dumps(json.load(sys.stdin), indent=2))"
```

**Required fields:**
- `account_id` — unique slug for this server (e.g., `launchpad`, `my-tools`, `code-server`)
- `server_url` — the MCP server endpoint URL

**Optional fields:**
- `auth_token` — bearer token for authentication
- `label` — human-readable display name

**Response:**
```json
{
  "success": true,
  "accountId": "my-mcp-server",
  "server_url": "http://localhost:3001/mcp",
  "message": "MCP account \"my-mcp-server\" registered. Call POST /v1/mcp/my-mcp-server/discover ..."
}
```

### Step 2: Discover Tools

Connect to the MCP server and list all available tools. This also shows a diff against any existing configs on disk (useful for refreshes).

```bash
sulla/v1/mcp/my-mcp-server/discover \
  -H "Content-Type: application/json" \
  | python3 -c "
import sys, json
data = json.load(sys.stdin)
if not data['success']:
    print(f\"Error: {data['error']}\")
    sys.exit(1)
print(f\"Discovered {data['toolCount']} tools:\")
for t in data['tools']:
    schema = t.get('inputSchema', {})
    props = list(schema.get('properties', {}).keys())
    req = set(schema.get('required', []))
    params = ', '.join(f\"{p}{'*' if p in req else ''}\" for p in props)
    print(f\"  - {t['name']}: {t['description']}\")
    if params:
        print(f\"      params: {params}\")
diff = data.get('diff', {})
if diff.get('added'):
    print(f\"\\nNew tools: {', '.join(diff['added'])}\")
if diff.get('removed'):
    print(f\"Removed tools: {', '.join(diff['removed'])}\")
"
```

**Response:**
```json
{
  "success": true,
  "accountId": "my-mcp-server",
  "toolCount": 5,
  "tools": [
    {
      "name": "search_docs",
      "description": "Search documentation by query",
      "inputSchema": {
        "type": "object",
        "properties": {
          "query": { "type": "string", "description": "Search query" }
        },
        "required": ["query"]
      }
    }
  ],
  "diff": { "added": ["search_docs"], "removed": [], "changed": [] }
}
```

**Review the tools list.** If it looks correct, proceed to finalize.

### Step 3: Finalize (Generate YAML Configs)

Write the integration configuration files. This creates a directory at `~/sulla/integrations/mcp-{accountId}/` with one auth YAML and one endpoint YAML per tool.

```bash
sulla/v1/mcp/my-mcp-server/finalize \
  -H "Content-Type: application/json" \
  | python3 -c "import sys, json; print(json.dumps(json.load(sys.stdin), indent=2))"
```

**Response:**
```json
{
  "success": true,
  "accountId": "my-mcp-server",
  "dirName": "mcp-my-mcp-server",
  "endpointCount": 5,
  "toolNames": ["search_docs", "create_item", "list_items", "get_item", "delete_item"]
}
```

After finalization, the integration config loader is automatically reloaded. The tools are immediately available.

### Step 4: Verify

Confirm the tools appear in the tools listing:

```bash
sulla/v1/tools/list?search=my-mcp-server' | python3 -c "
import sys, json
data = json.load(sys.stdin)
for t in data['tools']:
    print(f\"{t['slug']} ({t['accountId']}): {len(t.get('endpoints', []))} endpoints\")
    for ep in t.get('endpoints', []):
        print(f\"  - {ep['name']}: {ep['description']}\")
"
```

Now call any tool using the standard pattern:

```bash
sulla/v1/tools/{accountId}/{slug}/{endpoint}/call \
  -H "Content-Type: application/json" \
  -d '{"params": {"query": "hello"}}'
```

---

## Refreshing Tools (When the MCP Server Changes)

If the MCP server adds, removes, or changes tools, refresh the configs:

```bash
sulla/v1/mcp/my-mcp-server/refresh \
  -H "Content-Type: application/json" \
  | python3 -c "
import sys, json
data = json.load(sys.stdin)
diff = data.get('diff', {})
print(f\"Refreshed: {data['endpointCount']} endpoints\")
if diff.get('added'): print(f\"  Added: {', '.join(diff['added'])}\")
if diff.get('removed'): print(f\"  Removed: {', '.join(diff['removed'])}\")
"
```

This re-connects to the server, re-discovers tools, regenerates YAML files, and reloads the config loader.

---

## Removing an MCP Integration

To remove the generated YAML configs for an MCP account:

```bash
sulla/v1/mcp/my-mcp-server/configs \
  -H "Content-Type: application/json" \
  | python3 -c "import sys, json; print(json.dumps(json.load(sys.stdin), indent=2))"
```

This deletes the `~/sulla/integrations/mcp-{accountId}/` directory and reloads the config loader.

---

## What Gets Generated

For an MCP server with account ID `my-server` and 3 tools, the system creates:

```
~/sulla/integrations/mcp-my-server/
  mcp-my-server.v1-auth.yaml     <- auth config with transport: mcp
  search-docs.v1.yaml             <- endpoint for "search_docs" tool
  create-item.v1.yaml             <- endpoint for "create_item" tool
  list-items.v1.yaml              <- endpoint for "list_items" tool
```

### Auth YAML (auto-generated)
```yaml
api:
  name: mcp-my-server
  version: v1
  provider: mcp
  base_url: http://localhost:3001/mcp
  transport: mcp                    # Routes through MCPBridge, not fetch
mcp:
  account_id: my-server
  synced_at: "2026-03-25T12:00:00.000Z"
auth:
  type: bearer
  client_secret: "${MCP_AUTH_TOKEN}"
  token_storage: local
  refresh_automatically: false
```

### Endpoint YAML (auto-generated per tool)
```yaml
endpoint:
  name: search_docs
  description: Search documentation by query
  path: /tools/call
  method: POST
  auth: required
  transport: mcp                    # Routes through MCPBridge
body_params:
  query:
    type: string
    required: true
    description: Search query
```

The `transport: mcp` flag is the key — it tells ConfigApiClient to delegate to MCPBridge instead of making an HTTP fetch. The `body_params` are mapped from the MCP tool's `inputSchema`.

---

## How It Differs from REST Integrations

| Aspect | REST Integration | MCP Integration |
|--------|-----------------|-----------------|
| How you create it | Write YAML by hand | Auto-generated from `listTools()` |
| Transport | HTTP fetch | MCP SDK (StreamableHTTP/SSE) |
| Auth config | `transport: rest` (default) | `transport: mcp` |
| Endpoint path | Actual URL path (`/contacts`) | Always `/tools/call` |
| Parameters | `query_params`, `path_params`, `body_params` | `body_params` only (from inputSchema) |
| Pagination | Supported | Not applicable |
| Call routing | ConfigApiClient → fetch | ConfigApiClient → MCPBridge → MCP SDK |

---

## Troubleshooting

**"No MCP client for account"** — The server isn't connected. Register it first (Step 1), then discover (Step 2).

**"StreamableHTTP failed, retrying with SSE"** — The server doesn't support StreamableHTTP. The system auto-falls back to SSE transport. If the URL ends in `/sse`, SSE is used directly.

**Discovery returns 0 tools** — The server connected but exposed no tools. Verify the MCP server is configured to expose tools (check its `listTools` implementation).

**Tools appear in discover but not in `/v1/tools/list`** — You need to finalize first (Step 3). Discovery just previews; finalize writes the YAML configs.

---

## Discover Source (Auto-Detect MCP Servers)

When you don't have a clean URL and credentials — e.g. you have a `.mcp.json` file, a `docker-compose.yml`, or a URL that might need Docker port resolution — use the **discover-source** endpoint. It parses the source, resolves Docker networking, probes the server, and optionally registers + finalizes everything in one call.

```bash
sulla/v1/mcp/discover-source \
  -H "Content-Type: application/json" \
  -d '{
    "file_path": "/path/to/.mcp.json",
    "server_name": "wordpress",
    "auto_register": true
  }' | python3 -c "import sys, json; print(json.dumps(json.load(sys.stdin), indent=2))"
```

### Supported input types

**Raw URL** — provide `url` and optionally `auth_token`:
```json
{ "url": "http://localhost:3001/mcp", "auth_token": "my-token", "auto_register": true }
```

**.mcp.json file** — provide `file_path` and optionally `server_name` (for multi-server files):
```json
{ "file_path": "/path/to/.mcp.json", "server_name": "wordpress", "auto_register": true }
```

**docker-compose.yml** — provide `file_path` and optionally `server_name` (service name):
```json
{ "file_path": "/path/to/docker-compose.yml", "server_name": "mcp-wordpress", "auto_register": true }
```

### Request fields

| Field | Required | Description |
|-------|----------|-------------|
| `url` | One of url/file_path | Raw MCP server URL |
| `file_path` | One of url/file_path | Path to `.mcp.json` or `docker-compose.yml` |
| `auth_token` | No | Bearer token (overrides anything found in the source file) |
| `account_id` | No | Desired account slug (auto-generated if omitted) |
| `label` | No | Human-readable display name |
| `server_name` | No | Which server/service to use in multi-entry files |
| `auto_register` | No | If `true`, register + finalize in one call |

### Success response

```json
{
  "success": true,
  "resolved_url": "http://localhost:3001/mcp",
  "transport": "streamable-http",
  "suggested_id": "wordpress",
  "suggested_label": "Wordpress",
  "tool_count": 71,
  "tools": [ ... ],
  "source_type": "docker-compose",
  "source_details": {
    "file": "/path/to/docker-compose.yml",
    "service": "mcp-wordpress",
    "container": "merchantprotocol-mcp-wordpress",
    "internal_port": 7391,
    "host_port": 3001
  },
  "registered": true,
  "finalized": true,
  "dir_name": "mcp-wordpress"
}
```

### Error response (with actionable suggestions)

```json
{
  "success": false,
  "probes": [
    {
      "url": "http://localhost:3001/mcp",
      "transport": "streamable-http",
      "error": "401 Unauthorized",
      "suggestion": "Authentication failed. Provide a valid auth_token."
    }
  ],
  "partial": {
    "resolved_url": "http://localhost:3001/mcp",
    "source_type": "docker-compose",
    "missing_credentials": ["WORDPRESS_APP_PASSWORD"]
  }
}
```

### What it resolves automatically

- **Docker port mappings** — translates internal container ports to host-accessible ports via `docker port`
- **Environment variables** — resolves `${...}` references from `.env` files, compose env blocks, or `process.env`
- **Transport detection** — tries StreamableHTTP first, falls back to SSE
- **Docker-internal hostnames** — detects non-routable hosts (e.g. `https://app`) and tries localhost alternatives
- **Stdio servers** — detects `command: npx` servers, looks for running Docker containers wrapping them via supergateway

---

## API Reference

| Method | Endpoint | Purpose |
|--------|----------|---------|
| `POST` | `/v1/mcp/discover-source` | Auto-detect MCP server from URL, .mcp.json, or docker-compose.yml |
| `POST` | `/v1/mcp/register` | Register credentials (account_id, server_url, auth_token, label) |
| `POST` | `/v1/mcp/{accountId}/discover` | Connect + list tools + diff against existing configs |
| `POST` | `/v1/mcp/{accountId}/finalize` | Write YAML configs + reload config loader |
| `POST` | `/v1/mcp/{accountId}/refresh` | Re-discover tools + regenerate configs |
| `DELETE` | `/v1/mcp/{accountId}/configs` | Remove generated YAML directory |

---

## Tips

- **Choose descriptive account IDs** — `code-search`, `doc-server`, `team-tools` — they become the integration directory name
- **Discover before finalize** — always preview the tools list before writing configs
- **Refresh after server updates** — if the MCP server deploys new tools, call refresh to regenerate
- **One account per server** — each MCP server URL should be its own account; don't reuse account IDs
- **Local servers** — TLS certificate verification is automatically skipped for `localhost`, `127.0.0.1`, `*.local`, and `*.internal` hostnames
