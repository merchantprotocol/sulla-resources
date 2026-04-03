# load_skill

Load detailed instructions for a skill by name. Skills are specialized instruction sets that teach the agent how to handle specific domains — browser automation, debugging, lead generation, video creation, etc.

```bash
sulla skills/load_skill '{"skillName":"browser-automation"}'
```

| Param       | Type   | Required | Description                    |
|-------------|--------|----------|--------------------------------|
| `skillName` | string | yes      | Skill slug (directory name)    |

## How Skills Work

Skills live in `~/sulla/skills/`. Each skill directory contains a `SKILL.md` with detailed instructions. When loaded, the skill's instructions are injected into the agent's context, giving it specialized knowledge for that domain.

Skills are normally loaded automatically based on trigger keywords in the user's message. You can also load them manually when you need specific expertise.

## When to Load a Skill

- You're about to do browser automation → load `browser-automation`
- You need to debug a complex issue → load `debugging-methodology`
- You're working on a marketing plan → load `marketing-plan`
- You're setting up n8n workflows → load relevant n8n skills

Check `~/sulla/skills/` for the full list of available skills.
