---
slug: shopify
name: shopify-admin
type: proxy
auth_type: bearer
---

# shopify-admin Integration

## How It Works

This integration is a **transparent pass-through proxy**. You provide the HTTP method, path, and body. The system automatically injects the base URL and credentials from the vault. You never handle tokens or API keys directly.

## Usage

Use the `exec` tool to run `sulla` commands:

```
sulla <account_id>/shopify '<json_body>'
```

**JSON body fields:**

| Field     | Type   | Required | Description                                      |
|-----------|--------|----------|--------------------------------------------------|
| `method`  | string | yes      | HTTP method: GET, POST, PUT, PATCH, DELETE        |
| `path`    | string | yes      | API path (e.g. `/repos`, `/users`)               |
| `body`    | object | no       | Request body for POST/PUT/PATCH                   |
| `query`   | object | no       | Query string parameters as key-value pairs        |
| `headers` | object | no       | Extra headers (auth is already injected for you)  |

## Finding Your Account ID

```bash
sulla vault/list_accounts '{"account_type":"shopify"}'
```

The `account_id` marked `★ ACTIVE` is the default account.

## Examples

### List resources
```bash
sulla <account_id>/shopify '{"method":"GET","path":"/"}'
```

### Create a resource
```bash
sulla <account_id>/shopify '{
  "method": "POST",
  "path": "/resource",
  "body": {"key": "value"}
}'
```

## Troubleshooting

| Error                | Fix                                                              |
|----------------------|------------------------------------------------------------------|
| `UNAUTHENTICATED`    | Check credentials via `vault_read_secrets` tool         |
| `fetch failed`       | Verify the service is running and base_url is correct            |
| `Cannot connect`     | Ensure Sulla Desktop is running                                  |
