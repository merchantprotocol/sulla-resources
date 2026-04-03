# Proxy Call

Make authenticated API calls to any configured integration. Credentials are injected from the vault automatically — you never handle tokens or API keys directly.

## Usage

```bash
sulla <account_id>/<slug> '<json>'
```

| Field     | Type   | Required | Description                                      |
|-----------|--------|----------|--------------------------------------------------|
| `method`  | string | yes      | HTTP method: GET, POST, PUT, PATCH, DELETE        |
| `path`    | string | yes      | API path (e.g. `/repos`, `/rest/companies`)       |
| `body`    | object | no       | Request body for POST/PUT/PATCH                   |
| `query`   | object | no       | Query string parameters as key-value pairs        |
| `headers` | object | no       | Extra headers (auth is already injected)          |

## How It Works

1. `sulla` sends your request to the Sulla Desktop proxy
2. The proxy resolves the base URL and credentials from the encrypted vault
3. Your request is forwarded to the upstream API with auth injected
4. The raw response comes back (status code, body, errors — everything)

## Discovering Integrations

Find what account_id to use:
```bash
sulla vault/list_accounts '{"account_type":"twenty"}'
```

Check what credentials are stored:
```bash
sulla vault/read_secrets '{"account_type":"twenty"}'
```

## MCP Tools

MCP servers use a three-part path:
```bash
sulla <account_id>/mcp/<tool_name> '<json>'
```

Example:
```bash
sulla wordpress/mcp/list_posts '{}'
sulla dataripple/mcp/ping '{}'
```

## Examples

```bash
# Twenty CRM
sulla local_merchant_protocol/twenty '{"method":"GET","path":"/rest/companies"}'

# GitHub (if configured)
sulla my_github/github '{"method":"GET","path":"/user/repos"}'

# Any REST API with credentials in the vault
sulla <account_id>/<slug> '{"method":"POST","path":"/api/resource","body":{"key":"value"}}'
```

## Error Handling

The proxy returns the exact upstream status code and response body. Common errors:

| Status | Meaning                                          |
|--------|--------------------------------------------------|
| 401    | Credentials invalid or expired                   |
| 403    | Insufficient permissions                         |
| 404    | Endpoint doesn't exist at that path              |
| 500    | Upstream server error                            |

If you get auth errors, check credentials with `sulla vault/read_secrets`.
