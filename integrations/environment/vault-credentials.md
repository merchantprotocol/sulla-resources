# Vault & Credential Management

Encrypted credential store (AES-256-GCM) for websites and integrations.

## AI Access Levels

Each credential has an AI access level set by the user:

| Level | What you can do |
|-------|----------------|
| `none` | Credential is invisible to you — you can't see it exists |
| `metadata` | You can see it exists (name, URL) but can't read secrets |
| `autofill` | You can use it to log into websites via `vault/autofill` (default) |
| `full` | You can read all values including passwords and API keys |

## Tools

### vault/list
List all credentials you have access to. Respects AI access levels.
```bash
sulla vault/list '{}'
```

### vault/list_accounts
List accounts for a specific integration.
```bash
sulla vault/list_accounts '{"account_type":"slack"}'
```

### vault/read_secrets
Show credential status. Secrets masked by default — use `include_secrets: true` with `full` AI access.
```bash
sulla vault/read_secrets '{"account_type":"twenty"}'
```

### vault/set_credential
Store or update a credential. Encrypted in vault, never appears in conversation after.
```bash
sulla vault/set_credential '{"account_type":"twenty","property":"bearer_token","value":"sk-xxx"}'
```

### vault/set_active_account
Switch which account is active for an integration.
```bash
sulla vault/set_active_account '{"account_type":"slack","account_id":"work_slack"}'
```

### vault/is_enabled
Check if an integration is connected.
```bash
sulla vault/is_enabled '{"account_type":"github"}'
```

### vault/autofill
Fill login forms on websites using stored credentials. Password goes directly to browser — never in conversation.
```bash
sulla vault/autofill '{"origin":"https://github.com"}'
```

## Workflow

1. Need to log into a website? → `vault/autofill`
2. Need an API key for an integration? → `vault/read_secrets`
3. Need to see what's available? → `vault/list`
4. Need to store a new credential? → `vault/set_credential`
5. Credential not found? → Tell the user to add it to the vault
6. Access level too low? → Tell the user to upgrade the AI access level

## Rules

- Never hardcode credentials — always use the vault
- Never expose credentials in chat responses
- If a credential doesn't exist or access is denied, inform the user — don't try to work around it
