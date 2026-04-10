---
slug: business-onboarding-existing
title: Existing Business Onboarding
tags: [skill, onboarding, business, existing, empathy-map]
triggers: []
category: conversation
author: sulla-desktop
---

# Existing Business Onboarding

This skill onboards an existing business into Sulla. The user has already confirmed they have an existing business and may have provided a website URL. The conversation collects business context and builds a comprehensive empathy map from the business owner's perspective.

---

## Philosophy

- This is a conversation, not a form. Ask naturally, follow the owner's energy.
- Business owners know their audience deeply — draw it out of them.
- If they don't know an answer, don't force it. Acknowledge it and move on.
- Their answers won't always match the question — **you** categorize responses into the right empathy map sections. Listen for signals across everything they say.
- Keep it warm and human. No labels, no section headers read aloud, no meta-commentary.
- Document everything the business owner tells you, as you go, into `~/sulla/identity/business/business-owner-perspective-empathy-map.md`

---

## Conversation Flow

### Step 1: Collect the Website

If the router skill already collected a website URL, skip this step.

Otherwise:

> "Do you have a website for your business? Drop the URL here and I'll use it to learn about your business. If you don't have one, just say so — we'll work from what you tell me."

Record the URL (or note "none"). It gets saved in the onboarding document. The heartbeat agent will pick it up later and run website research in the background — you do NOT need to spawn a sub-agent here.

---

### Step 2: Empathy Map Interview

Walk through the following topics conversationally. You do NOT need to ask every sub-question — use them as probes when the lead question doesn't surface enough. Group related topics when natural. Adapt to what they've already told you.

**The answers from this entire conversation get categorized into the empathy map document. Listen for signals in everything they say — an answer to one question often contains data for three other sections.**

---

#### 2a: Target Audience

> "Tell me about the kind of customer you're targeting. Walk me through the different types of buyers you serve."

Probes:
- If you have multiple audiences, which one brings in the most money?
- Are you selling to consumers, businesses, or both?
- Where are they on the budget scale — price-sensitive or willing to invest?

**Extract:** All audience segments, the primary/most valuable audience, buyer characteristics.

---

#### 2b: Desire

> "What transformation does your ideal customer want most? What's the big outcome they're chasing?"

Probes:
- If they could only get one thing from you, what would it be?
- Is it about time, money, freedom, status, safety — what's the core driver?

**Extract:** All desires, the ONE big desire. Frame as: "You want to [desire]."

---

#### 2c: The Offer

> "What is it that you're offering this audience? What do people get when they work with you, and how does it work?"

Probes (only if needed):
- What kind of results do your customers typically get?
- Is this your core offering?

**Extract:** Offer description, how it works, results people get.

---

#### 2d: Prevailing Knowledge (The Big Mistake)

The prevailing knowledge is what most people in the industry believe to be the first steps to getting the outcome they want.

As an example, most people looking to buy a house think they need to first contact a Realtor, but they're wrong. 
The Realtor is likely only going to want to deal with them if they have first secured their financing.

> ask: "What do most of your customers think they need to do FIRST to achieve the outcome they're after, but they're actually wrong about?"

Probes:
- Is there a common first step that your audience takes in your industry?
- What's the myth or misconception about that process?

**Extract:** The prevailing (wrong) knowledge, the big mistake buyers make.

---

#### 2e: External Problems

> "What's the main problem standing between them and that outcome? What are they struggling with before they find you?"

Probes:
- Is this a problem they created or one imposed on them (regulations, market, etc.)?
- Have they tried to solve it before? What happened?

**Extract:** All problems, the ONE big problem. Frame as: "The problem is [problem]."

---

#### 2f: Emotional Pains

> "How does that problem make them feel? When they're stuck in it — what's the emotional weight?"

Probes:
- Are they frustrated, overwhelmed, angry, helpless?
- Does it affect their confidence or relationships?

**Extract:** All emotional pains, the strongest pain(s). Frame as: "This makes you feel [emotion]."

---

#### 2g: Fears and Worries

> "What's the worst-case scenario they're afraid of? What keeps them up at night about this?"

Probes:
- Is there a financial fear? A reputation fear? A personal fear?
- What happens if they never fix it?

**Extract:** All fears, the most prevalent fear. Frame as a fear statement.

---

#### 2h: The Cost of Inaction

> "If they do nothing — if they just stay where they are — what does that cost them? How bad does it get?"

**Extract:** Cost of inaction, lost opportunity, deterioration trajectory.

---

#### 2i: Urgent Crisis

> "What's the most urgent thing they need solved right now? The thing that can't wait?"

**Extract:** The most pressing crisis combining pain + fear + urgency.

---

#### 2j: Market Gap

We asked about the prevailing knowledge and the big mistake. We now understand what problems people face and why they
continue to fail to get the transformation they want.

> "Why is that wrong? What's the truth that most people miss?"

Probes:
- Can you back that up with evidence or experience?
- What have you seen happen when people follow the conventional wisdom?

**Extract:** Why the prevailing knowledge fails, factual evidence.

---

#### 2k: Unique Selling Proposition

> "So what's your approach? What do you do differently that actually gets results?"

Probes:
- Is there a method, framework, or process you follow?
- What makes your way better than what everyone else is doing?

**Extract:** The USP / proprietary method that overcomes the market gap.

---

#### 2l: Authority

> "What gives you the credibility to say that? Experience, certifications, track record — what proves your method works?"

**Extract:** Authority signals — experience, credentials, results, social proof.

---

#### 2m: Empathy

> "Have you been through this problem yourself, or what made you decide to solve it for others?"

Probes:
- Is there a personal story behind why you started this?
- What do you believe about your customers' right to have this solved?

**Extract:** Empathy statement. Frame as: "We believe [statement]."

---

#### 2n: Objections

> "When you pitch what you do, what pushback do you get? What do people say when they hesitate?"

Probes:
- Are these about price, trust, timing, or something else?
- What objections do they think but never say out loud?

**Extract:** Stated objections, unspoken hesitations.

---

### Step 3: Confirm and Write

Summarize the key points:

> "Here's what I'm hearing about your business and your audience — [brief summary]. Did I get that right?"

Let them correct anything. Then write/update the empathy map file.

---

### Step 4: Activate the Playbook

After writing both output files, activate the business onboarding project so the heartbeat agent picks up the remaining work:

1. **Create the project directory:** `~/sulla/projects/onboarding/`
2. **Copy the playbook template** from `~/sulla/resources/skills/onboarding/playbook.md` to `~/sulla/projects/onboarding/playbook.md`
3. **Update the project copy** — mark the Phase 1 human tasks as `[x]` with today's date
4. **Append to `~/sulla/projects/ACTIVE_PROJECTS.md`:**

```markdown
---

## Business Onboarding

| Field | Value |
|-------|-------|
| Status | ACTIVE |
| Project | onboarding |
| Priority | Business |
| Current State | Phase 1 human tasks complete — empathy map done, heartbeat tasks ready |
| Next Action | Heartbeat: run website research and customer empathy map |
| Blocker | None |
```

The heartbeat agent will now pick up the project on its next cycle and begin running Phase 1 heartbeat tasks (website research, customer perspective empathy map), then continue through Phase 2+.

---

## Output Files

### 1. Business Owner Perspective Empathy Map

Write to `~/sulla/identity/business/business-owner-perspective-empathy-map.md`:

```markdown
---
id: business-empathy-map
type: business
source: onboarding-conversation
date: [YYYY-MM-DD]
updated: [YYYY-MM-DD]
---

# Business Owner Perspective — Empathy Map

## The Offer
**What they offer:** [description]
**How it works:** [description]
**Results people get:** [description]

## Target Audience
**All audiences identified:**
- [audience 1]
- [audience 2]
- [audience 3]

**Primary (most valuable) audience:** [the one they starred]

## Desire
**All desires:**
- [desire 1]
- [desire 2]

**The ONE Big Desire:** You want to [desire].

## External Problems
**All problems:**
- [problem 1]
- [problem 2]

**The ONE Big Problem:** The problem is [problem].

## Emotional Pains
**All emotional pains:**
- [pain 1]
- [pain 2]

**The Strongest Pain:** This makes you feel [emotion].

## Fears and Worries
**All fears:**
- [fear 1]
- [fear 2]

**The Most Prevalent Fear:** [fear statement]

## The Cost of Inaction
[What happens if they do nothing]

## Urgent and Pressing Crisis
[The most pressing crisis]

## Prevailing Knowledge (The Big Mistake)
**What buyers think they need to do:** [the wrong approach]

## Market Gap
**Why the prevailing knowledge is wrong:** [factual evidence]

## Unique Selling Proposition
**The proprietary method:** [USP that overcomes the market gap]

## Authority
[Experience, certifications, track record, proof]

## Empathy
**We believe** [empathy statement]

## Objections
**Stated objections:**
- [objection 1]
- [objection 2]

**Unspoken hesitations:**
- [hesitation 1]

## Hurdles
[What holds them back]

## Benefits of Accomplished Desires
[What life looks like when they succeed]

## Reasons to Ultimately Commit
[What puts them over the fence]

## Day-to-Day Complaints
[What they complain about with friends and family]

## Why and What (Marketing Topics)
[Topics to discuss — why and what, not how]

## Raw Quotes
[5-10 direct quotes that capture the business owner's voice, perspective, and conviction]
```

**Sections the owner couldn't answer should be omitted entirely — don't leave blank placeholders.**

### 2. Business Onboarding Document

Write to `~/sulla/identity/business/onboarding.md`:

```markdown
---
id: business-onboarding
name: [Business Name]
type: business
source: onboarding-conversation
date: [YYYY-MM-DD]
updated: [YYYY-MM-DD]
website: [URL or "none"]
---

# Business Onboarding — [Business Name]

## Overview
- **Name:** [business name]
- **Website:** [URL or none]
- **Industry:** [industry/vertical]
- **Stage:** [early/growing/established/pivoting]
- **Business Model:** [how they make money]

## What They Offer
[Products and/or services, described in their words]

## Who They Serve
[Target customers/clients, market segments]

## How They Make Money
[Revenue model, pricing, key revenue streams]

## Immediate Priorities
[What they want Sulla to help with first]

## Raw Quotes
[3-5 direct quotes from the conversation]
```

**This file hides the business onboarding card in the UI.** It must always be written when onboarding completes.

---

## Rules

1. **Always write both output files** when the conversation completes.
2. **Always activate the playbook** (Step 4) after writing files — this is what triggers the heartbeat to continue the work.
3. **Categorize, don't parrot.** The owner's answers will bleed across sections — it's your job to put each piece of information in the right place in the empathy map.
4. **Don't force answers.** If they don't know, skip that section. An honest gap is better than a fabricated answer.
5. **Don't spawn sub-agents.** Website research and customer empathy map are heartbeat tasks — the heartbeat handles them after activation.
6. **Don't read section headers aloud.** The user should feel like they're having a strategy conversation, not filling out a form.
7. **Always speak like a human.** Warm, conversational, natural. No system text, no meta-commentary.
8. **Omit empty sections from the empathy map.** Only include what the owner actually provided.
9. **Quote them.** Their exact words are more valuable than your paraphrase. Capture direct quotes throughout.
