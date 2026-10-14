---
slug: azure_devops
name: azure-devops-rest-v7
type: proxy
auth_type: basic
---

# Azure DevOps integration

Store the complete organization URL, for example `https://dev.azure.com/acme`, in `base_url`. Typical calls look like:

```bash
sulla <account_id>/azure_devops '{"method":"GET","path":"/_apis/projects?api-version=7.1-preview.1"}'
sulla <account_id>/azure_devops '{"method":"GET","path":"/acme/widget/_apis/git/repositories?api-version=7.1"}'
sulla <account_id>/azure_devops '{"method":"GET","path":"/acme/widget/_apis/build/builds?api-version=7.1"}'
```

Find the account with `sulla vault/list_accounts '{"account_type":"azure_devops"}'`.

Azure DevOps PAT auth is HTTP Basic with an empty username and the PAT as the password. The current proxy in `pkg/rancher-desktop/main/chatCompletionsServer.ts` (`handleProxyCall`) only resolves `bearer_token` and `api_key`. To make this manifest fully operational, add resolution of `basic_username` and `basic_password` there and set `headers.Authorization = 'Basic ' + Buffer.from(`${username}:${password}`).toString('base64')` before the bearer/API-key fallback. No app code is changed in this resources PR.
