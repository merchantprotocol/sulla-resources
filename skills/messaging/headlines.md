---
slug: headlines
title: Marketing Headlines
tags: [skill, marketing, headlines, messaging, messaging]
triggers: ["create headlines", "marketing headlines", "headline generator"]
category: marketing
author: sulla-desktop
---

# Marketing Headlines

This skill generates marketing headlines for campaigns using the empathy map data. Headlines are the first thing prospects see — they must immediately resonate with the audience's desire and sophistication level.

---

## Input

Read the following files for context:
- `~/sulla/identity/business/business-owner-perspective-empathy-map.md`
- `~/sulla/identity/business/customer-perspective-empathy-map.md` (if available)
- `~/sulla/identity/business/onboarding.md`

---

## Audience Sophistication

Headline effectiveness depends entirely on the audience's sophistication level within the niche. The more sophisticated (having heard more offers and being familiar with them), the more information the audience requires.

### Sophistication Levels

**Level 1 — Unaware:** Audience doesn't know they have a problem.
- Lead with the problem or a story.

**Level 2 — Problem Aware:** They know the problem but not the solution.
- Lead with the pain/problem, then introduce the benefit.

**Level 3 — Solution Aware:** They know solutions exist but haven't picked one.
- Lead with your differentiator or mechanism.

**Level 4 — Product Aware:** They know your product but haven't bought.
- Lead with proof, urgency, or a special offer.

**Level 5 — Most Aware:** They know you and just need a reason to act now.
- Lead with the deal, reminder, or new angle.

### Headline Formulas

Generate headlines at each level of specificity:

1. **(Benefit)** — simplest, for less sophisticated audiences
2. **(Benefit) in (timeframe)** — adds urgency
3. **(Benefit) in just (timeframe) days. Using (mechanism).** — most specific, for sophisticated audiences

---

## Process

1. Determine the likely sophistication level of the target audience based on the empathy map (industry, competition, how much the audience has been marketed to).
2. Pull the ONE Big Desire, ONE Big Problem, Strongest Pain, USP/mechanism from the empathy map.
3. Generate **10-15 headlines** across the formulas above, varying:
   - Benefit-led vs. pain-led vs. curiosity-led
   - Short (5-8 words) vs. medium (10-15 words)
   - Different sophistication levels
4. Mark which sophistication level each headline targets.
5. Recommend the top 3 for immediate use.

---

## Output

Write to `~/sulla/identity/business/headlines.md`:

```markdown
---
id: marketing-headlines
type: business
source: empathy-map
date: [YYYY-MM-DD]
updated: [YYYY-MM-DD]
---

# Marketing Headlines

## Audience Sophistication Assessment
**Level:** [1-5] — [label]
**Reasoning:** [why this level, based on industry/audience]

## Headlines

### Benefit-Led
1. [headline] — Level [N]
2. [headline] — Level [N]
3. [headline] — Level [N]

### Pain-Led
1. [headline] — Level [N]
2. [headline] — Level [N]
3. [headline] — Level [N]

### Curiosity / Mechanism-Led
1. [headline] — Level [N]
2. [headline] — Level [N]
3. [headline] — Level [N]

### With Timeframe
1. [headline] — Level [N]
2. [headline] — Level [N]

### With Mechanism
1. [headline] — Level [N]
2. [headline] — Level [N]

## Top 3 Recommended
1. **[headline]** — Best for [context/channel]
2. **[headline]** — Best for [context/channel]
3. **[headline]** — Best for [context/channel]
```

---

## After Writing

Update the business playbook at `~/sulla/projects/onboarding/playbook.md` — mark this step as complete with today's date.

---

## Rules

1. **Every headline must come from empathy map data.** Don't invent benefits or claims not supported by the owner's or customer's words.
2. **Match sophistication to the audience.** Don't use Level 1 headlines for a saturated market.
3. **Use the audience's language.** Pull actual phrases from the empathy map raw quotes when possible.
4. **No filler headlines.** Every one should be usable in a real campaign.
