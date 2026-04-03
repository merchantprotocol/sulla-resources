---
slug: prd-perspectives
title: PRD Perspectives & Critic Rounds
tags: [skill, prd, project, critique, refinement, planning]
triggers: ["prd critique", "prd review", "refine prd", "prd perspectives"]
category: project
author: sulla-desktop
---

# PRD Perspectives — Critic Rounds

After the PRD Manager produces a Project Resource Document, the PRD goes through sequential rounds of critique. Each round is a fresh agent that sees ONLY the current version of the PRD plus one critique prompt. The PRD is rewritten after each round.

The goal is not to tear the PRD apart — it's to pressure-test every section so the final document is airtight before work begins.

---

## Critic Round 1: Goal Alignment Audit

Read the PRD and the identity/goals context that was provided to the PRD Manager. For every section of the PRD, ask:

- Does the "Ultimate Project Goal" directly advance the human's stated goals? Which specific goals does it serve? If it doesn't clearly connect to any goal, why does this project exist?
- Do the user stories reflect real pain points from the identity files, or are they generic placeholders?
- Are the Must Haves actually must-haves for the human's goals, or are some of them nice-to-haves wearing a must-have label?
- Is the project sized appropriately for where the human is right now — their capacity, their current workload, their energy level?

Flag every misalignment. Then rewrite the ENTIRE PRD with goal alignment tightened at every level.

---

## Critic Round 2: Scope & Feasibility Check

Examine the PRD for scope creep, unrealistic commitments, and missing constraints:

- Is this project trying to do too many things at once? Could it be split into smaller, shippable phases?
- Are there dependencies that aren't acknowledged — credentials, APIs, external approvals, skills the human doesn't have yet?
- Does the execution checklist represent a realistic path from start to finish, or does it skip hard steps?
- Are there implicit assumptions about timeline, effort, or complexity that should be made explicit?
- If there are existing active projects, does this project compete for the same resources/attention?

List every scope or feasibility concern. Then rewrite the ENTIRE PRD with scope tightened and feasibility issues addressed — either by reducing scope, adding phases, or explicitly calling out risks.

---

## Critic Round 3: Architecture & Technical Soundness

Review the PRD's technical and architectural decisions:

- Is the architecture section specific enough for a worker to actually implement, or is it hand-wavy?
- Are there technology choices that should be justified? Why this stack/approach over alternatives?
- Does the architecture account for the human's existing infrastructure, tools, and patterns?
- Are integration points clearly defined — what talks to what, what format, what protocol?
- Is there a clear data model or is the PRD vague about what data flows where?
- Are error handling, edge cases, and failure modes addressed?

Flag every architectural gap. Then rewrite the ENTIRE PRD with technical decisions clarified and gaps filled.

---

## Critic Round 4: Assumption Audit

List every assumption baked into this PRD. Flag which ones are weak or unverified. Specifically:

- Assumptions about the human's available time and capacity
- Assumptions about technical feasibility ("this API supports X" — does it actually?)
- Assumptions about credentials, accounts, or access being available
- Assumptions about external services remaining stable or free
- Assumptions about the human's technical skill level for this particular project
- Assumptions about other projects not conflicting with this one
- Assumptions about what "done" looks like — is the definition of done actually measurable?

Now rewrite the ENTIRE PRD with those assumptions either defended with evidence from the context provided, explicitly flagged as risks, or removed entirely.

---

## Critic Round 5: Completeness & Quality Ratchet

Rate this PRD honestly on a scale of 1-10. Then ask:

- What would a 10/10 version of this PRD look like?
- Is anything missing that a worker would need to ask about before they could start?
- Are the user stories specific enough to test against?
- Does the execution checklist have clear, verifiable completion criteria for each step?
- Is the credential resolution table complete — are there integrations mentioned in the architecture that aren't in the table?
- Would someone reading this PRD for the first time understand exactly what to build, why, and how to verify it's done?

Now write the 10/10 version. Every section should be complete enough that a worker can execute without asking clarifying questions.

---

## Critic Round 6: Coherence & Final Polish

Read the PRD end to end as a single document. Find:

- Internal contradictions — does the architecture contradict the requirements? Do Must Haves contradict Nice-to-Haves?
- Vague claims — anything that says "should handle" or "will support" without specifying how
- Unsupported assertions — any claim about feasibility, timeline, or effort without backing
- Disconnected sections — does every Must Have appear in the execution checklist? Does every architecture component serve a user story?
- Naming consistency — are components, services, and concepts named consistently throughout?
- Checklist completeness — does the execution checklist cover everything in Must Haves + Should Haves, and are items in a logical order?

Eliminate every gap, contradiction, and vague claim. Produce the final clean version of the PRD.

---

## Usage Notes

- The project-management workflow loads this skill at the start
- The PRD Manager produces the initial PRD — it never sees these critic prompts
- Critic agents work sequentially — each receives the output of the previous round
- Each critic rewrites the ENTIRE PRD, not just the section they're critiquing
- The number of critic rounds can be adjusted by the parent workflow's orchestrator
- The final output of the last critic round IS the finished PRD — the critic writes it back to PROJECT.md
