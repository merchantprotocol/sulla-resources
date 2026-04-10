# Sulla Environment Overview

## Architecture

Sulla Desktop is an Electron app running on macOS. It manages:

- **Agent graphs** — LLM-driven tool loops that handle tasks, conversations, and automation
- **Express server** (port 3000) — HTTP API for tool calls and proxy requests
- **Lima VM** — Linux virtual machine running Docker, containers, and the `sulla` CLI
- **Vault** — AES-256-GCM encrypted credential store for API keys, tokens, and passwords
- **WebSocket service** — real-time communication between agents, UI, and external services
- **IntegrationService** — manages accounts, credentials, and connection status for all integrations

### What Runs Where

| Component | Where | How to access |
|-----------|-------|---------------|
| Electron app, Express server, agent graphs | Host macOS | Internal — you don't call these directly |
| Docker containers (Twenty CRM, n8n, postgres, etc.) | Lima VM | `sulla docker/*` tools |
| `sulla` CLI | Lima VM (PATH at `/usr/local/bin/sulla`) | Via `exec` tool |
| sulla-daemon (socat relay) | Lima VM | Automatic — keeps TCP connection warm |
| Vault, IntegrationService, database | Host (Electron process) | `sulla vault/*` tools |
| Browser (Playwright) | Host macOS | `sulla playwright/*` and `sulla chrome/*` tools |

### Communication Flow

```
Agent → exec tool → sulla CLI (Lima VM) → daemon socket or direct TCP → Express server (host:3000) → proxy/tool handler → upstream API
```

Credentials are resolved inside the Express server from the encrypted vault. They never travel through the CLI or appear in tool call arguments.

## Directory Structure

```
~/sulla/                              # Agent home directory
├── agents/                           # Sub-agent configs (agentId = folder name)
│   ├── dreaming-protocol/            # Radius — primary planning agent
│   ├── sulla/                        # Main conversation agent
│   ├── code-researcher/              # Code analysis agent
│   ├── thinker/                      # Deep thinking agent
│   └── ...
├── skills/                           # Skill library (one folder per skill)
│   ├── browser-automation/SKILL.md
│   ├── debugging-methodology/SKILL.md
│   ├── marketing-plan/SKILL.md
│   └── ...
├── workflows/                        # Sulla Workflow YAML files
│   ├── production/                   # Ready to run
│   ├── draft/                        # In development
│   └── archive/                      # Retired
├── projects/                         # Project workspaces (PROJECT.md per project)
├── integrations/                     # Integration configs and documentation
│   ├── <slug>/                       # Per-integration folder
│   │   ├── <slug>.v*-auth.yaml       # Auth config (base_url, auth type)
│   │   └── INTEGRATION.md            # Usage docs with sulla examples
│   ├── tools/                        # Tool reference docs (one .md per tool)
│   │   └── browser/                  # Browser-specific tool docs and skills
│   └── environment/                  # System documentation (this folder)
├── conversations/                    # Past conversations for training
├── logs/                             # Debug logs
├── daily-logs/                       # Structured daily observation logs
│   └── YYYY-MM-DD/
│       ├── {domain}/observations/
│       └── {domain}/thinking/
└── identity/                         # Persistent identity & goals
    ├── human/                        # identity.md + goals.md
    ├── business/                     # identity.md + goals.md
    ├── world/                        # identity.md + goals.md
    └── agent/                        # identity.md + goals.md
```

### Codebase

```
~/.sulla-desktop/                     # Sulla Desktop source (git repo)
  → https://github.com/merchantprotocol/sulla-desktop
  → https://sulladesktop.com
```

## Docker Runtime

Docker daemon runs inside the Lima VM. All containers, images, and networks are managed there.

- `exec` tool runs commands inside the Lima VM where Docker is available
- Host filesystem is mounted via virtiofs — you can read/write macOS files
- Containers reach the host at `host.docker.internal`
- Docker tools: `sulla docker/ps`, `sulla docker/run`, `sulla docker/exec`, `sulla docker/logs`, `sulla docker/stop`, `sulla docker/rm`, `sulla docker/pull`, `sulla docker/images`, `sulla docker/build`

### Running Services (typical)

| Service | Container | Port | Purpose |
|---------|-----------|------|---------|
| Twenty CRM | twenty-crm-server | 30207 | CRM database |
| Twenty Postgres | twenty-crm-postgres | — | Twenty's database |
| n8n | n8n | 5678 | Workflow automation |
| Redis | redis | 6379 | Key-value store, caching |
| PostgreSQL | postgres | 5432 | Primary database |

Check running containers: `sulla docker/ps '{}'`

## sulla CLI

The `sulla` CLI is the primary interface for all tool calls from inside the VM. It's a shell script at `/usr/local/bin/sulla`.

```bash
# Proxy calls (authenticated API pass-through)
sulla <account_id>/<slug> '{"method":"GET","path":"/api/endpoint"}'

# Internal tools (by category)
sulla <category>/<tool_name> '{"param":"value"}'

# MCP tools
sulla <account_id>/mcp/<tool_name> '{"param":"value"}'
```

Tool categories: `meta`, `docker`, `github`, `playwright`, `chrome`, `rdctl`, `redis`, `pg`, `n8n`, `slack`, `kubectl`, `lima`, `calendar`, `extensions`, `vault`, `agents`, `bridge`, `skills`, `workflow`, `computer-use`

## Finding Things

Use `file_search` to discover anything in `~/sulla/`:
```bash
sulla meta/file_search '{"query":"CRM integration","dirPath":"/home/user/sulla"}'
sulla meta/file_search '{"query":"video generation","dirPath":"/home/user/sulla/skills"}'
```

`file_search` is semantic — describe what you want, not the exact filename.

## Projects

A folder becomes a project when it contains a `PROJECT.md` file. Projects live at `~/sulla/projects/`.

## Integrations

140+ pre-configured integrations. Each uses the proxy pattern — you send method + path + body, the system injects credentials. Integration docs at `~/sulla/integrations/<slug>/INTEGRATION.md`.

Key integrations with active credentials:
- **Twenty CRM** — `sulla local_merchant_protocol/twenty '...'`
- **Slack** — `sulla slack/send_message '...'`
- **WordPress** (MCP) — `sulla wordpress/mcp/list_posts '...'`

Check what's connected: `sulla vault/list_accounts '{"account_type":"<slug>"}'`

## Memory System

Observational memories persist across conversations. Use them for facts about the user, project context, and patterns.

```bash
sulla meta/add_observational_memory '{"priority":"🟡","content":"..."}'
sulla meta/remove_observational_memory '{"id":"aB3x"}'
```

Memories are injected into every agent context. Keep them concise and relevant.

## Error Recovery

| Problem | Solution |
|---------|----------|
| `Cannot connect to Sulla Desktop` | sulla-daemon not running: `sulla-daemon start` |
| `WORKSPACE_NOT_FOUND` on API call | Bearer token invalid — regenerate via API or UI |
| `No base_url configured` | Store it: `sulla vault/set_credential '{"account_type":"...","property":"base_url","value":"https://..."}'` |
| `fetch failed` | Target service not running — check with `sulla docker/ps '{}'` |
| Tool not found | Check tool name and category — `sulla <category>/<tool>` |
| Auth errors on proxy | Check credentials: `sulla vault/read_secrets '{"account_type":"..."}'` |
