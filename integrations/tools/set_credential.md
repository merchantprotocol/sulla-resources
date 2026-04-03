# vault/set_credential

Store or update a credential for an integration account. The value is encrypted in the vault and never appears in conversation after this call.

```bash
sulla vault/set_credential '{"account_type":"twenty","property":"bearer_token","value":"sk-xxx"}'
```

## Parameters

| Param          | Type   | Required | Description                                           |
|----------------|--------|----------|-------------------------------------------------------|
| `account_type` | string | yes      | Integration slug (e.g. `twenty`, `slack`, `github`)    |
| `property`     | string | yes      | Credential name (`bearer_token`, `api_key`, `base_url`) |
| `value`        | string | yes      | Value to store (will be encrypted)                    |
| `account_id`   | string | no       | Specific account (defaults to active)                 |

## Common properties

| Property       | Description                              |
|----------------|------------------------------------------|
| `bearer_token` | API key or bearer token for auth         |
| `api_key`      | API key (alternative to bearer)          |
| `base_url`     | Base URL of the service                  |

## Behavior

- Setting `bearer_token`, `api_key`, `access_token`, or `password` automatically marks the integration as connected.
- All subsequent proxy calls for this integration will use the stored credentials.
