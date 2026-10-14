---
slug: forgejo
name: forgejo-rest-v1
type: proxy
auth_type: bearer
---

# Forgejo / Codeberg integration

The default upstream is Codeberg at `https://codeberg.org/api/v1`. Store that complete API URL in `base_url`; a self-hosted Forgejo URL must also end in `/api/v1`.

```bash
sulla <account_id>/forgejo '{"method":"GET","path":"/user"}'
sulla <account_id>/forgejo '{"method":"GET","path":"/repos/acme/widget"}'
sulla <account_id>/forgejo '{"method":"GET","path":"/repos/acme/widget/pulls"}'
```

Find the account with `sulla vault/list_accounts '{"account_type":"forgejo"}'`.
