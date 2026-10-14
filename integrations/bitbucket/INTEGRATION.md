---
slug: bitbucket
name: bitbucket-cloud-rest-v2
type: proxy
auth_type: bearer
---

# Bitbucket Cloud integration

The proxy injects a stored repository or workspace access token as `Authorization: Bearer …` and forwards requests to `https://api.bitbucket.org/2.0`.

```bash
sulla <account_id>/bitbucket '{"method":"GET","path":"/user"}'
sulla <account_id>/bitbucket '{"method":"GET","path":"/repositories/acme"}'
sulla <account_id>/bitbucket '{"method":"GET","path":"/repositories/acme/widget/pullrequests"}'
```

Find the account with `sulla vault/list_accounts '{"account_type":"bitbucket"}'`.

Atlassian API tokens authenticate with the Atlassian account email and token using HTTP Basic, not bearer auth. The proxy currently only injects `bearer_token` or `api_key`; supporting Atlassian API tokens requires adding `basic_username` and `basic_password` resolution plus `Authorization: Basic ${base64(username:password)}` in `pkg/rancher-desktop/main/chatCompletionsServer.ts` inside `handleProxyCall`.
