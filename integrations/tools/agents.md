# Agent Tools

Spawn sub-agents to handle tasks independently. Sub-agents run their own LLM loop with access to tools, and can execute in parallel or async.

## spawn_agent

```bash
sulla agents/spawn_agent '{"agentName":"researcher","task":"Find pricing for competitor products","async":true}'
```

| Param       | Type    | Required | Description                                     |
|-------------|---------|----------|-------------------------------------------------|
| `agentName` | string  | yes      | Name of the agent config to use                 |
| `task`      | string  | yes      | Task description for the sub-agent              |
| `async`     | boolean | no       | Run in background (default: false)              |

When `async: true`, the tool returns immediately with a job ID. Use `check_agent_jobs` to poll for results.

When `async: false` (default), the tool blocks until the sub-agent completes and returns its result.

## check_agent_jobs

```bash
sulla agents/check_agent_jobs '{"jobIds":["job-123","job-456"]}'
```

| Param    | Type     | Required | Description                         |
|----------|----------|----------|-------------------------------------|
| `jobIds` | string[] | yes      | Job IDs from async spawn_agent calls |

## Available Agents

Agent configs live in `~/sulla/agents/`. Each has a `config.yaml` defining its name, description, allowed tools, and skills. Check that directory for what's available.

## When to Use Sub-Agents

- **Research tasks** — spawn an agent to investigate while you continue other work
- **Parallel execution** — spawn multiple agents for independent tasks
- **Specialized work** — delegate to an agent with specific skills/tools
