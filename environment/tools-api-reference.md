# Sulla Tools Reference

All tools are accessible through the `sulla` CLI. Use the `exec` tool to run `sulla` commands.

## Command Format

```bash
# Proxy call — authenticated API pass-through
sulla <account_id>/<slug> '<json>'

# Internal tool — by category
sulla <category>/<tool_name> '<json>'

# MCP tool
sulla <account_id>/mcp/<tool_name> '<json>'

# Bare tool name — routes to integrations category
sulla <tool_name> '<json>'
```

## Tool Categories

| Category       | Examples                                    |
|----------------|---------------------------------------------|
| `vault`        | list, list_accounts, read_secrets, set_credential, autofill |
| `docker`       | ps, run, exec, logs, stop, rm, images, pull, build |
| `github`       | git_status, git_commit, git_push, create_issue, create_pr |
| `playwright`   | browser_tab, click_element, set_field, get_page_text |
| `chrome`       | notify_user, search_history, agent_storage, manage_cookies |
| `slack`        | send_message, search_users, user, thread |
| `redis`        | get, set, del, hget, hset, incr, expire |
| `pg`           | query, queryone, queryall, count, execute, transaction |
| `n8n`          | validate_workflow, patch_workflow, diagnose_webhook |
| `meta`         | exec, file_search, read_file, add_observational_memory |
| `rdctl`        | info, list_settings, set, shell, extension, snapshot |
| `calendar`     | list, create, update, cancel, delete, list_upcoming |
| `kubectl`      | apply, describe, delete |
| `lima`         | list, shell, start, stop, create, delete |
| `extensions`   | list_extension_catalog, install_extension |
| `agents`       | spawn_agent, check_agent_jobs |
| `skills`       | load_skill |
| `workflow`     | execute_workflow, validate_sulla_workflow |
| `bridge`       | get_human_presence, update_human_presence |
| `computer-use` | computer (screenshot, click, type, scroll) |

## Examples

```bash
# Docker
sulla docker/ps '{}'
sulla docker/logs '{"container":"my-app","tail":"50"}'

# Git
sulla github/git_status '{}'
sulla github/git_commit '{"message":"fix: resolve bug"}'

# Vault
sulla vault/list '{}'
sulla vault/list_accounts '{"account_type":"twenty"}'

# Browser
sulla playwright/browser_tab '{"action":"upsert","url":"https://example.com"}'
sulla playwright/get_page_text '{"assetId":"browser_123"}'

# Proxy (Twenty CRM)
sulla local_merchant_protocol/twenty '{"method":"GET","path":"/rest/companies"}'

# MCP
sulla wordpress/mcp/list_posts '{}'

# Redis
sulla redis/set '{"key":"test","value":"hello"}'
sulla redis/get '{"key":"test"}'

# Notifications
sulla chrome/notify_user '{"title":"Done","message":"Build complete"}'

# Memory
sulla meta/add_observational_memory '{"priority":"🟡","content":"User prefers dark mode"}'
```

## Documentation

- **Per-tool docs:** `~/sulla/integrations/tools/`
- **Browser tools:** `~/sulla/integrations/tools/browser/`
- **Per-integration docs:** `~/sulla/integrations/<slug>/INTEGRATION.md`
- **Environment docs:** `~/sulla/integrations/environment/`
