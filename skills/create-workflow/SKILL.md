---
schemaversion: 1
slug: create-workflow
title: "Create a Workflow / Routine"
section: "Standard Operating Procedures"
category: "Automation Authoring"
tags:
  - skill
  - workflow
  - routine
  - automation
  - authoring
  - scaffold
  - validate
order: 5
locked: true
author: seed
---

# Create a Workflow / Routine

**Triggers**: Human says "build me a workflow", "create a routine", "make a workflow",
"build an automation", "set up a recurring job", "scaffold a workflow", "I want to automate",
"let's make a routine that …", "put this on a schedule", "turn this into a workflow".

Scaffold, validate, install-to-DB, and iteratively edit a Sulla workflow (a.k.a. routine)
so the human watches it take shape in the Routines editor as you build it.

---

## The contract (end-to-end)

Every workflow-authoring turn follows this exact flow. Don't skip steps; don't reorder
them. The validate-before-install and install-before-open gates exist because skipping
them leaves half-wired state the user sees as "Sulla said it made a workflow and there's
nothing there."

1. **Confirm scope in one turn.** Don't start writing YAML until you've agreed on:
   - Trigger: `manual` | `schedule` | `chat` | `calendar` | `heartbeat`
   - Steps in plain English (what each node does, in order)
   - Branches / loops / parallel runs (if any)
   - Failure mode: abort on error / continue / retry
   - If a schedule: cron shape (daily, weekly, hourly, every N minutes)
2. **Scaffold** the routine dir with `marketplace/scaffold`.
3. **Draft** the YAML — edit the scaffolded `routine.yaml` in place.
4. **Validate twice** — `meta/validate_sulla_workflow` for graph structure, then
   `marketplace/validate` for kind-schema. Both must return zero errors.
5. **Install to DB** as `draft` with `workflow/import_workflow`. This creates the row
   the routines area reads from.
6. **Display it as an artifact** — run `sulla workflow/display_workflow '{"slug":"<slug>"}'`.
   The tool reads the routine.yaml and publishes it to the chat artifact sidebar.
   The workflow artifact pane renders the node graph (same component that renders
   live executions), using the routine's own `position: {x,y}` coordinates. The
   artifact is deduped by workflow name — repeat calls for the same slug update
   the same sidebar card in place, so the user watches the routine grow as you edit.
7. **Iterate**: edit YAML → re-validate → re-import → re-run `display_workflow`.
   The sidebar updates in place; no new card per edit.
8. **Finalize** by flipping `status` to `production` on the last `import_workflow` call.
   Only `production` status gets picked up by the scheduler.

---

## Tool mapping

| Step | Tool | Notes |
|------|------|-------|
| Scaffold dir + skeleton manifest | `sulla marketplace/scaffold '{"kind":"workflow","slug":"<slug>"}'` | Creates `~/sulla/routines/<slug>/routine.yaml` with a minimal trigger + agent skeleton. Never `mkdir` + `write_file` by hand — the scaffolder produces the correct shape. |
| Inspect / edit the YAML | `read_file` → `write_file` (or `Edit`) | File at `~/sulla/routines/<slug>/routine.yaml` |
| Graph validate | `sulla meta/validate_sulla_workflow '{"filePath":"/Users/<user>/sulla/routines/<slug>/routine.yaml"}'` | Checks node↔category mapping, required config fields, edge structure, reachability, trigger presence. |
| Kind-schema validate | `sulla marketplace/validate '{"kind":"workflow","slug":"<slug>"}'` | Checks manifest-level: top-level keys, slug/id/directory agreement, version metadata. |
| Install / upsert to DB | `sulla workflow/import_workflow '{"slug":"<slug>","status":"draft"}'` | Dual-writes disk + DB. Re-run after every material edit — it's idempotent. |
| Display in the chat artifact sidebar | `sulla workflow/display_workflow '{"slug":"<slug>"}'` | Publishes the routine.yaml as a `workflow` artifact in the right pane. Uses the routine's own node coordinates — what the user sees IS the graph. Deduped by workflow name; re-run after each edit to update the card in place. |
| Run the finished workflow | `sulla meta/execute_workflow '{"workflowId":"<slug>"}'` | Only after validation is clean AND status has been flipped to `production` (or the user explicitly asks to test as draft). When execution starts, runtime state overlays (active/done/error coloring) layer onto the same artifact — nodes light up as they fire. |

---

## The artifact-display step

Goal: the user stays in the chat window and watches the workflow grow in the right-side
artifact pane as you build it.

### The mechanism

`sulla workflow/display_workflow '{"slug":"<slug>"}'` reads `~/sulla/routines/<slug>/routine.yaml`
and publishes the full document as a `workflow_document` event. The chat frontend's
PersonaAdapter opens (or updates in place) a workflow artifact in the sidebar, keyed
by the workflow's display name. The artifact payload IS the routine — same
`node.position.{x,y}`, same `node.data.{label,subtype,config}`, same `edges[].source/target`.
Zero field mapping; what you wrote in the YAML is what the user sees.

### Dedup by name

The same workflow name on re-emit updates the same artifact. Repeat calls after each
edit mutate the sidebar card in place; they don't stack new artifacts.

### Canonical authoring loop

```
edit routine.yaml
  → sulla meta/validate_sulla_workflow '{"filePath":"…"}'
  → sulla marketplace/validate '{"kind":"workflow","slug":"…"}'
  → sulla workflow/import_workflow '{"slug":"…","status":"draft"}'
  → sulla workflow/display_workflow '{"slug":"…"}'
```

The artifact card in the sidebar re-renders with the new nodes/edges the moment
`display_workflow` succeeds.

### When the workflow runs

Execution is the same artifact — the backend's per-node events layer runtime state
(active / done / error) onto the existing nodes (matched by id). The user sees the
same graph they've been watching, now with traveling pulses and completion marks
as each step fires. No second artifact, no context switch.

### Hard rules

- Always `import_workflow` **before** `display_workflow` — the display tool reads from
  disk, but showing a graph that isn't in the DB means "run it" won't work yet.
- Re-run `display_workflow` after every material edit. It's cheap (one YAML read)
  and keeps the sidebar honest.
- Never use `ui/open_tab '{"mode":"routines"}'` as the "show it" step during authoring.
  That's a separate editor tab; we're building an in-chat artifact experience.

---

## Minimum viable routine.yaml shape

The scaffolder produces this — don't write it from scratch:

```yaml
id: <slug>
name: "<Display Name>"
description: "<one-line summary>"
version: 1
createdAt: "<ISO timestamp>"
updatedAt: "<ISO timestamp>"
enabled: true
_status: draft

nodes:
  - id: trigger-1
    type: workflow
    position: { x: 100, y: 100 }
    data:
      subtype: manual                     # or schedule | chat | calendar | heartbeat
      category: trigger
      label: "Start"
      config: {}

  - id: agent-1
    type: workflow
    position: { x: 400, y: 100 }
    data:
      subtype: agent
      category: agent
      label: "Do the thing"
      config:
        agentId:                 "sulla-desktop"
        agentName:               "Sulla"
        additionalPrompt:        ""
        orchestratorInstructions: "Run the task and return a result."
        successCriteria:         "Task completed with a non-empty result."
        completionContract:      "Return a structured result object."

edges:
  - id:           edge-1
    source:       trigger-1
    target:       agent-1
    sourceHandle: null
    targetHandle: null
    label:        ""
    animated:     true

viewport: { x: 0, y: 0, zoom: 1 }
```

Every node needs `type: workflow`, `position`, and `data: { subtype, category, label, config }`.
Agent nodes MUST carry all six config fields above — missing any is a validator error.

---

## Iteration pattern

Editing a node config? Follow this exact sequence:

1. `Edit` or `write_file` the YAML
2. `sulla meta/validate_sulla_workflow '{"filePath":"…"}'` — fix any errors
3. `sulla workflow/import_workflow '{"slug":"<slug>","status":"draft"}'` — re-upsert
4. (Optional) Tell the human what you changed so they can refresh the editor if needed
   — the DB watcher will pick it up on its own, usually within a second.

Never skip step 2 — schema errors pass eyeball review and fail at the first fork.

---

## When the user says "ship it" / "make it live"

1. Run the full validation pass one final time.
2. Re-run `import_workflow` with `status: "production"`.
3. If the trigger is a schedule, the `WorkflowSchedulerService` picks it up within a
   tick and registers the cron. No manual restart needed.
4. Confirm back to the user: slug, status, next scheduled invocation (if applicable).

---

## Hard rules

- **Scaffold before you write.** Don't hand-roll the directory. The scaffolder produces
  a well-formed skeleton; your job is to fill in the actual steps.
- **Both validators pass before install.** Skipping either is how broken workflows
  end up in the DB and silently no-op at runtime.
- **Open the editor.** The human should SEE the draft. A workflow that lives only in
  the DB is invisible to them.
- **Status `draft` during authoring, `production` to ship.** Never leave a half-edited
  workflow at `production` status — the scheduler will try to run it.
- **Re-import after every material edit.** YAML changes don't automatically reach the
  DB. `import_workflow` is the bridge.

---

## Reference docs (sulla-docs)

- `workflows/schema.md` — full YAML spec
- `workflows/node-types.md` — every subtype + required config fields per subtype
- `workflows/examples.md` — four complete working patterns
- `workflows/authoring.md` — deeper authoring, validation, debug, scheduling, restart
- `agent-patterns/validation.md` — never-ship-unverified contract across all artifacts

---

## What this skill does NOT cover

- **Function creation** — if the workflow needs a custom function, drop out of this
  skill and handle that with the function-creation path first, then resume.
- **Sub-agents / custom agents** — if a routine needs a brand-new agent persona,
  author that separately, commit, then reference its `agentId` from the routine.
- **n8n workflows** — different engine, different tools (`n8n/*`). Sulla routines
  and n8n workflows don't share a validator.
