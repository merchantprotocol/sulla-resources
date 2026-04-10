---
slug: business-onboarding
title: Business Onboarding Conversation
tags: [skill, onboarding, conversation, business, goals]
triggers: ["put sulla to work on my business", "business onboarding", "set up my business", "business setup", "get started with my business"]
category: conversation
author: sulla-desktop
---

# Business Onboarding Conversation

This skill guides a structured conversation to understand the user's business context so Sulla can begin working on it effectively. It determines whether this is a new or existing business and routes to the appropriate next skill.

---

## Philosophy

Keep it short and focused. This is a routing conversation — get the key facts, then hand off to the right skill.

- Be warm and conversational, not bureaucratic
- Don't over-ask — get what you need and move on
- The goal is to route to the right next step as quickly as possible

---

## Conversation Flow

### Step 1: New or Existing?

**Lead question:**
> "Great — let's get started! First, are we working with an existing business, or is this something you're building from scratch?"

Wait for the user's answer. They will say one of:
- **Existing business** → proceed to Step 2A
- **New business** → proceed to Step 2B

---

### Step 2A: Existing Business

**Lead question:**
> "Perfect. Do you have a website for the business? If so, drop the URL here — I'll take a look and use it to get up to speed faster. If not, just say so and we'll work from what you tell me."

Wait for the user's response. They will provide:
- A website URL → record it, then proceed to handoff
- "I don't have one" or similar → acknowledge, then proceed to handoff

**After collecting the response, load the existing business skill:**

```
~/sulla/resources/skills/onboarding/business-onboarding-existing.md
```

Pass along any context collected (the website URL if provided).

---

### Step 2B: New Business

**After confirming it's a new business, load the new business skill:**

```
~/sulla/resources/skills/onboarding/business-onboarding-new.md
```

---

## Rules

1. **Don't skip the routing question.** Always ask new vs. existing first.
2. **Don't interrogate.** This is a 2-question conversation at most before handing off.
3. **Always speak like a human.** Warm, conversational, natural. No system text, no labels, no meta-commentary.
4. **Pass context forward.** When loading the next skill, include everything the user has already told you so they don't repeat themselves.
5. **If the sub-skill file doesn't exist yet**, tell the user: "I've got the basics — the detailed business setup workflow is coming soon. For now, I've noted everything you've shared and I'll use it when the full workflow is ready." Then write what you collected to `~/sulla/identity/business/onboarding.md`.
6. **Always write `~/sulla/identity/business/onboarding.md`** when the conversation completes — this is what hides the onboarding card. The file should contain everything collected during this conversation.
