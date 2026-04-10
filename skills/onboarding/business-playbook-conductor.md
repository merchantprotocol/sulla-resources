---
slug: business-playbook-conductor
title: Business Playbook Conductor
tags: [skill, business, playbook, conductor, orchestration]
triggers: ["business playbook", "what's next for my business", "continue business setup", "business progress"]
category: orchestration
author: sulla-desktop
---

# Business Playbook Conductor

This skill documents how the business onboarding playbook is orchestrated across two actors: the **heartbeat agent** (autonomous background work) and **Sulla Desktop** (human-facing conversation).

---

## The Two Actors

### Heartbeat Agent (background, ~10 min cycle)
- Reads `~/sulla/projects/ACTIVE_PROJECTS.md` to find work
- Reads `~/sulla/projects/onboarding/playbook.md` to find the next step
- Executes **heartbeat tasks** only — never attempts human tasks
- Checks output file existence to determine what's unblocked
- Executes ONE task per cycle
- Updates playbook checkboxes and ACTIVE_PROJECTS.md after each task
- When blocked by a human task: updates ACTIVE_PROJECTS.md with `HUMAN ACTION REQUIRED`

### Sulla Desktop Agent (human-facing)
- Gets context via the Memory Recall Agent (SubconsciousMiddleware)
- Recall agent reads `ACTIVE_PROJECTS.md` on every user message
- When it sees `HUMAN ACTION REQUIRED` → injects into `<recall_context>`
- Main agent naturally surfaces it in conversation: "By the way, to keep your business setup moving, I need..."
- When user provides info → skill writes output file → heartbeat unblocks

---

## The Playbook

### Locations

- **Template:** `~/sulla/resources/skills/onboarding/playbook.md` — master copy, ships with Sulla, never modified
- **Active project:** `~/sulla/projects/onboarding/playbook.md` — live copy that tracks progress

### Structure

Each phase has two sections:
- **Human Tasks** — require user interaction, executed by Sulla Desktop
- **Heartbeat Tasks** — autonomous, executed by the heartbeat agent

Each row has: Status, Step, Skill path, Output File path, Dependencies.

### Status tracking

- `[ ]` = not started
- `[x] (YYYY-MM-DD)` = complete
- File existence is the **source of truth** — checkboxes are for readability
- A task is **available** when all its dependency output files exist
- A task is **blocked** when any dependency output file is missing

---

## Lifecycle

### 1. Activation (after Phase 1 human tasks)

The `business-onboarding-existing.md` skill handles activation when the empathy map interview completes:

1. Copies playbook template → `~/sulla/projects/onboarding/playbook.md`
2. Marks Phase 1 human tasks as `[x]`
3. Appends entry to `~/sulla/projects/ACTIVE_PROJECTS.md`

### 2. Heartbeat picks up Phase 1 heartbeat tasks

On the next cycle, heartbeat reads the playbook and finds:
- Website research — depends on `onboarding.md` (exists) → **available**
- Customer empathy map — depends on owner empathy map (exists) → **available**

Executes them, updates checkboxes.

### 3. Phase 2+ heartbeat tasks auto-chain

As Phase 1 heartbeat tasks complete, Phase 2 heartbeat tasks unblock:
- Headlines — depends on both empathy maps → runs when both exist
- Elevator pitch — depends on both empathy maps → runs in parallel with headlines
- Brand script — depends on elevator pitch → runs after
- etc.

### 4. Human tasks surface naturally

When the heartbeat hits Phase 2 human tasks (website access, blog test), it can't execute them. Instead it updates ACTIVE_PROJECTS.md:

```
| Next Action | HUMAN ACTION REQUIRED: Connect website to password vault for blog publishing |
```

The recall agent picks this up and the Sulla Desktop agent asks the user naturally.

### 5. Human completes task → heartbeat unblocks

User provides CMS credentials → skill writes `website-access.md` → heartbeat sees the file → continues with tasks that depended on it.

---

## Heartbeat Execution Rules

When the heartbeat picks up the onboarding project:

1. **Read** `~/sulla/projects/onboarding/playbook.md`
2. **Scan all phases** for heartbeat tasks (not just the current phase)
3. **For each heartbeat task:**
   - Check if the output file already exists → if yes, mark `[x]` and skip
   - Check if all dependency output files exist → if not, skip (blocked)
   - If available → **load the skill** at the path in the Skill column
   - Execute the skill — it reads its input files and writes its output file
4. **After execution:**
   - Update the playbook: change `[ ]` to `[x] (YYYY-MM-DD)` for the completed step
   - Update `updated` in the playbook frontmatter
5. **Check for human task blockers:**
   - If the next available step is a human task → update ACTIVE_PROJECTS.md:
     - `Next Action: HUMAN ACTION REQUIRED: [description of what's needed]`
   - If more heartbeat tasks are available → update ACTIVE_PROJECTS.md:
     - `Next Action: Heartbeat: [next task name]`
6. **One task per cycle.** Don't try to run the entire playbook in one heartbeat.

---

## ACTIVE_PROJECTS.md Entry Format

```markdown
---

## Business Onboarding

| Field | Value |
|-------|-------|
| Status | ACTIVE |
| Project | onboarding |
| Priority | Business |
| Current State | [what's been done so far] |
| Next Action | [Heartbeat: task name OR HUMAN ACTION REQUIRED: description] |
| Blocker | [None OR description of what's blocking] |
```

The heartbeat updates `Current State` and `Next Action` after each cycle.

---

## When Triggered by the User

If the user asks "what's next for my business" or similar:

1. Read `~/sulla/projects/onboarding/playbook.md`
2. Check file existence for all steps
3. Give concise status:
   > "Phase 1 is done. Phase 2 is in progress — 3 of 6 messaging tasks complete. I'm waiting on your website credentials to keep going."
4. If human tasks are needed, ask for them directly
5. Don't dump the whole playbook — summarize

---

## Rules

1. **File existence is truth.** Always verify output files, don't trust checkboxes alone.
2. **Heartbeat NEVER runs human tasks.** It only flags them in ACTIVE_PROJECTS.md.
3. **One task per heartbeat cycle.** Don't batch.
4. **Dependencies are the gate.** Never execute a task whose dependencies aren't met.
5. **Update state after every action.** Playbook checkboxes + ACTIVE_PROJECTS.md.
6. **Don't nag the user.** Surface human tasks naturally, don't repeat every conversation.
