---
slug: mcp
name: MCP
type: mcp
auth_type: varies
---

# MCP Integration

MCP (Model Context Protocol) servers are managed separately from proxy integrations. Each MCP server has its own account and connection. Use `sulla` to discover available MCP tools:

```bash
sulla vault/list_accounts '{"account_type":"mcp"}'
```
