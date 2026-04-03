# vault/is_enabled

Check whether a specific integration is connected and enabled.

```bash
sulla vault/is_enabled '{"account_type":"github"}'
```

## Parameters

| Param          | Type   | Required | Description                              |
|----------------|--------|----------|------------------------------------------|
| `account_type` | string | yes      | Integration slug (e.g. `github`, `slack`) |

## Response

Returns the enabled status, connection timestamps, and a list of accounts if multiple exist.
