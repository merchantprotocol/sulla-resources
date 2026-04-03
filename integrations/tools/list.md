# vault/list

List all saved website credentials in the vault. Shows website URLs and usernames for accounts where AI access is permitted. Passwords are never included.

```bash
sulla vault/list '{}'
```

## Parameters

None required.

## Response

Returns a list of saved website credentials with their URLs, usernames, and AI access levels.

```
- Local Twenty (20) CRM (local_twenty_20_crm): http://localhost:30207/ — user@example.com [AI access: full]
```

AI access levels:
- `full` — the model can read credentials programmatically
- `autofill` — credentials can only be injected into browser forms, not read
- `none` — no AI access
