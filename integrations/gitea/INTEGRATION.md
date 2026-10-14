---
slug: gitea
name: gitea-rest-v1
type: proxy
auth_type: bearer
---

# Gitea integration

Store the complete API base URL, including `/api/v1`, in `base_url`. Calls use a stored personal access token.

```bash
sulla <account_id>/gitea '{"method":"GET","path":"/user"}'
sulla <account_id>/gitea '{"method":"GET","path":"/repos/acme/widget"}'
sulla <account_id>/gitea '{"method":"GET","path":"/repos/acme/widget/pulls"}'
```

Find the account with `sulla vault/list_accounts '{"account_type":"gitea"}'`.
