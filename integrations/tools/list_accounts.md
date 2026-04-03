# vault/list_accounts

List all accounts configured for an integration. Shows labels, connection status, and which account is currently active.

```bash
sulla vault/list_accounts '{"account_type":"twenty"}'
```

## Parameters

| Param          | Type   | Required | Description                              |
|----------------|--------|----------|------------------------------------------|
| `account_type` | string | yes      | Integration slug (e.g. `twenty`, `slack`) |

## Response

Returns a list of accounts with their `account_id`, label, connection status, and which is marked `★ ACTIVE`.

```
Accounts for "twenty" (1):
- local merchant protocol (account_id: "local_merchant_protocol") | Connected ★ ACTIVE
```

The `account_id` marked `★ ACTIVE` is the default account used by proxy calls.
