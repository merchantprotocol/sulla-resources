# vault/autofill

Autofill a login form on the current browser tab with saved vault credentials. The password is injected directly into the browser — it never appears in conversation.

```bash
sulla vault/autofill '{"origin":"https://github.com"}'
```

## Parameters

| Param        | Type   | Required | Description                                      |
|--------------|--------|----------|--------------------------------------------------|
| `origin`     | string | no       | Website origin to match (e.g. `https://github.com`) |
| `account_id` | string | no       | Specific vault account ID to use                 |

Provide either `origin` (matches by website URL) or `account_id` (uses a specific vault entry). If neither is provided, the tool will fail.

## Requirements

The vault credential must have AI access set to `autofill` or `full`. Credentials with AI access set to `none` cannot be used.
