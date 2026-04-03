# vault/read_secrets

Show credential status for an integration account. By default, secret values are masked with `[VAULT PROTECTED]`. Set `include_secrets: true` to reveal values (requires the account's AI access level to be set to `full`).

```bash
sulla vault/read_secrets '{"account_type":"twenty"}'
```

## Parameters

| Param             | Type    | Required | Description                                |
|-------------------|---------|----------|--------------------------------------------|
| `account_type`    | string  | yes      | Integration slug (e.g. `twenty`, `github`)  |
| `account_id`      | string  | no       | Specific account (defaults to active)      |
| `include_secrets` | boolean | no       | Reveal secret values (default: false)      |

## Response

Shows each credential property: name, title, type, whether required, and its value (masked or revealed).

```
- API Key (bearer_token): [VAULT PROTECTED] (Required)
- Base URL (base_url): http://localhost:30207/ (Required)
```

## Reveal secrets

```bash
sulla vault/read_secrets '{"account_type":"twenty","include_secrets":true}'
```

Only works if the account's AI access level is `full`. If set to `autofill`, secrets remain masked.
