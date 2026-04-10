---
slug: sms-offer-drop
title: SMS Offer Drop
tags: [skill, marketing, sms, outreach, messaging]
triggers: ["sms offer", "text message offer", "sms drop"]
category: marketing
author: sulla-desktop
---

# SMS Offer Drop

This skill crafts an SMS offer drop message for cold/warm outreach. The benefit is the most important aspect. The rapport statement allows us to bypass their "spam" defenses — if it's not accurate, it has the opposite effect and people will object instead of reading.

---

## Input

Read:
- `~/sulla/identity/business/business-owner-perspective-empathy-map.md`
- `~/sulla/identity/business/headlines.md`
- `~/sulla/identity/business/onboarding.md`

---

## SMS Formula

```
My name is (Name), I found you (rapport).

(headline statement)

You can reach me on my cell (phone), but if this is a bad time to talk, would it be appropriate to setup a call sometime this week?
```

### Rapport Statement

The rapport statement must be accurate and specific. It tells the prospect how you found them and establishes a connection before the pitch. If it's wrong or generic, the whole message fails.

**Examples:**
- I found you on LinkedIn
- I received your information from (common friend)
- I was referred to you by (common friend)
- I saw your post about (topic) in (group)
- I came across your business on (platform)

### Headline Statement

Pull from the headlines already generated. Use the strongest benefit-led headline that fits SMS length constraints (keep the total message under 300 characters if possible).

---

## Process

1. Read the empathy map to understand the audience and offer.
2. Read the headlines file for pre-crafted headline statements.
3. Generate **3-5 SMS variations**:
   - Vary the rapport statement for different outreach contexts (LinkedIn, referral, cold list, event, etc.)
   - Vary the headline statement (benefit-led vs. pain-led)
   - Keep each total message under 300 characters when possible
4. Recommend the top version for each outreach context.

---

## Output

Write to `~/sulla/identity/business/sms-offer-drop.md`:

```markdown
---
id: sms-offer-drop
type: business
source: empathy-map + headlines
date: [YYYY-MM-DD]
updated: [YYYY-MM-DD]
---

# SMS Offer Drop

## LinkedIn Outreach
```
My name is [Name], I found you on LinkedIn.

[headline statement]

You can reach me on my cell [phone], but if this is a bad time to talk, would it be appropriate to setup a call sometime this week?
```

## Referral Outreach
```
My name is [Name], I received your information from [mutual connection context].

[headline statement]

You can reach me on my cell [phone], but if this is a bad time to talk, would it be appropriate to setup a call sometime this week?
```

## Cold List Outreach
```
[variation for cold outreach]
```

## Recommended Version
**Best for most situations:** [the strongest version]
**Why:** [reasoning based on audience sophistication]
```

---

## After Writing

Update the business playbook at `~/sulla/projects/onboarding/playbook.md` — mark this step as complete with today's date.

---

## Rules

1. **Rapport must be accurate.** Never use a rapport statement the business can't actually back up.
2. **Keep it short.** SMS is not the place for long copy.
3. **The headline does the heavy lifting.** The rapport just gets them to read it.
4. **Generate for real outreach contexts** the business actually uses.
