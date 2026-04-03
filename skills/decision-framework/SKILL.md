---
slug: decision-framework
title: Decision Framework — When to Act, Research, Plan, or Delegate
schemaversion: 1
tags: [decision, process, research, plan, execute, workflow, strategy, approach, methodology, phases]
triggers: ["how should I approach", "decision process", "should I research first", "planning", "execution strategy", "when to delegate", "full process", "skip to action", "how to handle", "complex task"]
created_at: 2026-04-01T00:00:00.000Z
updated_at: 2026-04-01T00:00:00.000Z
---

# Decision Framework

## Skip to Action When:

- You already know the answer (conversation, factual recall, opinion)
- You already know which tool to call and how (no discovery needed)
- The user is asking you to repeat or adjust something you just did

## Follow the Full Process When:

- You need to figure out *how* to do something
- You need to find the right tool, skill, or integration
- The task touches an external system you haven't used yet this session
- Multiple steps are involved and failure at any step would waste effort

**When in doubt:** start with a quick `file_search` — if it turns up exactly what you need, act on it. If not, run the full process.

## Full Process

### Phase 1: Research

Before acting, understand what you have available. Spawn sub-agents (`code-researcher`) in parallel for:

1. **Skills & tools** — `file_search` for related skills + search Tools API (`?search=<keyword>`) for integration endpoints. Return: matching skills, available API endpoints with call signatures.
2. **Workflows** — search for existing workflows that match the use case. If one exists, report it.
3. **Resource mapping** — check vault for credentials, search Tools API for the target service, identify access paths (API, database, browser). Return: access methods ranked by efficiency.
4. **External research** — only if internal research was insufficient, search the web via Playwright browser tools.

Wait for research results before proceeding.

### Phase 2: Plan

Based on research:
- Align the task with confirmed goals
- For substantial work: create a project, present the plan to the user, get confirmation
- For repeatable tasks: plan to create a workflow after completion
- If a matching workflow was found: execute it via `execute_workflow` and skip to Phase 4

### Phase 3: Execute

Attempt using this priority order. Move to the next level only after the current one fails:

1. **Pre-built API integrations** — use endpoints discovered in Phase 1 via Tools API. Auth handled automatically.
2. **Sub-agent with API details** — spawn a sub-agent with exact integration details (endpoints, auth, schemas) to work through the problem via API.
3. **Browser UI fallback** — if API approaches fail, spawn a sub-agent to accomplish the task through browser UI with Playwright tools.
4. **Reassess** — if all approaches fail, step back. Re-examine research, try a different angle. Clean up partial state before retrying.

### Phase 4: Follow-up

- If repeatable, create a workflow with the `create-workflow` skill
- Update observational memory with what you learned
- Report results to the user
- If you spot a goal-aligned automation opportunity, propose a scheduled workflow
