---
slug: docker-extensions
title: Docker Extensions Marketplace — Install, Manage, and Monitor Stacks
schemaversion: 1
tags: [docker, extensions, marketplace, compose, install, container, stack, lifecycle, resource]
triggers: ["extension", "extensions", "install extension", "docker compose", "marketplace", "stack", "container management", "running extensions", "extension logs", "start extension", "stop extension"]
created_at: 2026-04-01T00:00:00.000Z
updated_at: 2026-04-01T00:00:00.000Z
---

# Docker Extensions Marketplace

Pre-built Docker Compose stacks installable via the Extensions tools. Each extension is a self-contained service stack.

## Extension Tools

| Tool | Purpose |
|------|---------|
| `list_extension_catalog` | Browse all available extensions in the marketplace |
| `list_installed_extensions` | See what's currently installed and their status |
| `install_extension` | Install a new extension from the catalog |
| `uninstall_extension` | Remove an installed extension |

## Lifecycle Management

Once installed, extensions are managed via the Tools API:

```bash
# Start an extension
sulla/v1/tools/internal/extensions/start/call \
  -H "Content-Type: application/json" \
  -d '{"params": {"extensionId": "n8n"}}'

# Stop an extension
sulla/v1/tools/internal/extensions/stop/call \
  -H "Content-Type: application/json" \
  -d '{"params": {"extensionId": "n8n"}}'
```

Available lifecycle commands: `start`, `stop`, `restart`, `status`, `update`, `logs`

## Resource Management

Every running extension consumes CPU, memory, and disk on the user's machine.

**Rules:**
- Stop extensions when not actively in use
- Check `list_installed_extensions` before installing to avoid duplicates
- Monitor resource usage if the user reports performance issues
- Prefer starting an existing extension over installing a new one

## Common Extensions

Use `list_extension_catalog` to see what's available. Common categories include databases, monitoring tools, development utilities, and automation platforms.
