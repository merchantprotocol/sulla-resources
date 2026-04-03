# vault/set_active_account

Switch which account is active for an integration. All subsequent proxy calls and tool calls for this integration will use the active account's credentials.

```bash
sulla vault/set_active_account '{"account_type":"slack","account_id":"work_slack"}'
```

## Parameters

| Param          | Type   | Required | Description                              |
|----------------|--------|----------|------------------------------------------|
| `account_type` | string | yes      | Integration slug (e.g. `slack`, `gmail`)  |
| `account_id`   | string | yes      | Account to make active (from `list_vault_accounts`) |

## When to use

Use this when an integration has multiple accounts (e.g. personal Gmail and work Gmail) and you need to switch which one is used for API calls.
