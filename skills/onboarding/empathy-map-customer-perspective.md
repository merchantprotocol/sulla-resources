---
slug: empathy-map-customer-perspective
title: Empathy Map — Customer Perspective
tags: [skill, empathy-map, marketing, audience, research, customer]
triggers: []
category: research
author: sulla-desktop
---

# Empathy Map — Customer Perspective

This skill is run as an async sub-agent after the business owner completes their empathy map interview. It answers the same empathy map questions but from the **target audience's perspective** — using research, inference, and the owner's answers as context.

---

## Input

This sub-agent receives:
- The business owner's empathy map: `~/sulla/identity/business/business-owner-perspective-empathy-map.md`
- Website research (if available): `~/sulla/identity/business/products-and-services.md`, `~/sulla/identity/business/target-audience.md`, `~/sulla/identity/business/market-positioning.md`
- The empathy map question set: `~/sulla/resources/skills/onboarding/empathy-map-questions.md`

---

## Process

1. **Read the owner's empathy map** to understand the business, offer, and who the audience is.
2. **Read any website research documents** if they exist — these provide additional context about the business's positioning and messaging.
3. **For each question in the empathy map**, answer it from the customer's perspective:
   - What would the **customer** say their desire is? (This may differ from what the owner thinks.)
   - What problems does the **customer** experience and how would **they** describe them?
   - What emotions does the **customer** actually feel? (Not what the owner assumes.)
   - What fears keep the **customer** up at night — in **their** language?
   - What does the **customer** think the prevailing knowledge is? (They're living it.)
   - What objections does the **customer** have that they may never voice?
   - What do **customers** complain about to friends and family?

4. **Use web research** where helpful — look at forums, Reddit, review sites, social media, and industry discussions to find real customer voices for this type of business and audience.

5. **Flag discrepancies** between what the owner believes and what the customer likely experiences. These gaps are high-value insights for marketing.

---

## Output

Write to `~/sulla/identity/business/customer-perspective-empathy-map.md`

Use the empathy map document template from `~/sulla/resources/skills/onboarding/empathy-map-questions.md`.

Set the frontmatter:
- `id: customer-perspective-empathy-map`
- `source: research + owner-empathy-map`
- Title: `Customer Perspective — Empathy Map`

### Additional Section: Owner vs. Customer Gaps

At the bottom of the document, add:

```markdown
## Owner vs. Customer Gaps

[List specific areas where the owner's perspective differs from the likely customer perspective. These are the most valuable insights for improving marketing messaging.]

- **[Section]:** Owner says [X], but customers likely feel/think [Y] because [reason].
```

---

## Rules

1. **Research real customer voices.** Don't just rewrite the owner's answers in third person. Look for actual customer language — reviews, forum posts, social discussions.
2. **The customer's words matter more than your synthesis.** If you find real quotes or common phrasings, use them.
3. **Flag gaps honestly.** If the owner's view doesn't match reality, say so clearly. That's the whole point of doing this from both perspectives.
4. **Don't invent data.** If you can't find evidence for a section, say what's likely based on the business type and leave it clearly marked as inference.
5. **Omit empty sections.** Same rule — don't leave blank placeholders.
