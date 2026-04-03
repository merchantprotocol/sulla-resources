# Memory Tools

Store and remove observations from long-term memory. Memories persist across conversations and are injected into every agent context. Use them to record facts about the user, their preferences, project context, and patterns you notice.

## When to Add a Memory

- You learn something about the user's preferences or workflow
- You discover a fact about the project that isn't obvious from the code
- You notice a pattern that would help future conversations
- The user explicitly tells you to remember something

Keep memories concise — one sentence with enough context to be useful later. Duplicate observations are automatically detected and merged.

## add_observational_memory

```bash
sulla meta/add_observational_memory '{"priority":"🟡","content":"User prefers dark mode for all UIs"}'
```

| Param      | Type   | Required | Description                              |
|------------|--------|----------|------------------------------------------|
| `priority` | enum   | yes      | `🔴` critical, `🟡` normal, `⚪` low     |
| `content`  | string | yes      | One sentence — concise, include context  |

Priority guide:
- **🔴** — corrections, strong preferences, things that caused problems
- **🟡** — useful context, workflow preferences, project facts
- **⚪** — nice to know, minor observations

## remove_observational_memory

```bash
sulla meta/remove_observational_memory '{"id":"aB3x"}'
```

| Param | Type   | Required | Description                    |
|-------|--------|----------|--------------------------------|
| `id`  | string | yes      | 4-character ID of the memory   |

Use this when a memory is outdated, wrong, or no longer relevant.
