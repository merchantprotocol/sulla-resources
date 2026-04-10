---
id: business-playbook
type: business
source: onboarding
date: 2026-04-09
updated: 2026-04-09
---

# Business Playbook

This file tracks progress through the business setup and growth process. Each item links to the skill that produces it and the output file that marks it complete. The agent checks file existence to determine status.

**This is a template.** Do not modify this file directly.

**Activation — when Phase 1 human tasks complete:**
1. Copy this file to `~/sulla/projects/onboarding/playbook.md`
2. Update the copy — mark Phase 1 human tasks as `[x]` with completion dates
3. Add an entry to `~/sulla/projects/ACTIVE_PROJECTS.md`

The heartbeat picks up the project on its next cycle and begins running Phase 1 heartbeat tasks, then chains through Phase 2+. See `business-playbook-conductor.md` for full orchestration details.

**Task types:**
- `human` — Sulla Desktop asks the user during a conversation
- `heartbeat` — heartbeat agent runs autonomously in the background

---

## Phase 1: Business Foundation

### Human Tasks

| Status | Step | Skill | Output File | Depends On |
|--------|------|-------|-------------|------------|
| [ ] | Business onboarding | `~/sulla/resources/onboarding/business-onboarding.md` | `~/sulla/identity/business/onboarding.md` | — |
| [ ] | Empathy map — owner perspective | `~/sulla/resources/skills/onboarding/business-onboarding-existing.md` | `~/sulla/identity/business/business-owner-perspective-empathy-map.md` | Business onboarding |

### Heartbeat Tasks

| Status | Step | Skill | Output File | Depends On |
|--------|------|-------|-------------|------------|
| [ ] | Website research | `~/sulla/resources/skills/onboarding/business-website-research.md` | `~/sulla/identity/business/products-and-services.md` | Business onboarding (URL) |
| [ ] | Empathy map — customer perspective | `~/sulla/resources/skills/onboarding/empathy-map-customer-perspective.md` | `~/sulla/identity/business/customer-perspective-empathy-map.md` | Owner empathy map |

---

## Phase 2: Messaging & Brand Script

### Human Tasks

| Status | Step | Skill | Output File | Depends On |
|--------|------|-------|-------------|------------|
| [ ] | Website access & blog publishing | `~/sulla/resources/skills/onboarding/website-access.md` | `~/sulla/identity/business/blog-publishing-verified.md` | Business onboarding (URL) |

### Heartbeat Tasks

| Status | Step | Skill | Output File | Depends On |
|--------|------|-------|-------------|------------|
| [ ] | Headlines | `~/sulla/resources/skills/messaging/headlines.md` | `~/sulla/identity/business/headlines.md` | Both empathy maps |
| [ ] | SMS offer drop | `~/sulla/resources/skills/messaging/sms-offer-drop.md` | `~/sulla/identity/business/sms-offer-drop.md` | Headlines |
| [ ] | Elevator pitch | `~/sulla/resources/skills/messaging/elevator-pitch.md` | `~/sulla/identity/business/elevator-pitch.md` | Both empathy maps |
| [ ] | Brand script | `~/sulla/resources/skills/messaging/brand-script.md` | `~/sulla/identity/business/brand-script.md` | Both empathy maps, elevator pitch |
| [ ] | Cold call to action | `~/sulla/resources/skills/messaging/cold-cta.md` | `~/sulla/identity/business/cold-cta.md` | Brand script |
| [ ] | Mission statement | `~/sulla/resources/skills/messaging/mission-statement.md` | `~/sulla/identity/business/mission-statement.md` | Both empathy maps |

---

## Phase 3: Lead Generation Assets

### Human Tasks

| Status | Step | Skill | Output File | Depends On |
|--------|------|-------|-------------|------------|

### Heartbeat Tasks

| Status | Step | Skill | Output File | Depends On |
|--------|------|-------|-------------|------------|
| [ ] | Lead magnet design | `~/sulla/resources/skills/messaging/lead-magnet.md` | `~/sulla/identity/business/lead-magnet.md` | Brand script, empathy maps |
| [ ] | Landing page strategy | `~/sulla/resources/skills/messaging/landing-page.md` | `~/sulla/identity/business/landing-page-strategy.md` | Lead magnet, website research |
| [ ] | Email welcome sequence | `~/sulla/resources/skills/messaging/email-sequence.md` | `~/sulla/identity/business/email-welcome-sequence.md` | Lead magnet, brand script |

---

## Phase 4: Content Strategy

### Human Tasks

| Status | Step | Skill | Output File | Depends On |
|--------|------|-------|-------------|------------|

### Heartbeat Tasks

| Status | Step | Skill | Output File | Depends On |
|--------|------|-------|-------------|------------|
| [ ] | Content calendar | `~/sulla/resources/skills/messaging/content-calendar.md` | `~/sulla/identity/business/content-calendar.md` | Brand script, empathy maps |
| [ ] | Social media strategy | `~/sulla/resources/skills/messaging/social-strategy.md` | `~/sulla/identity/business/social-media-strategy.md` | Content calendar |
| [ ] | SEO keyword strategy | `~/sulla/resources/skills/messaging/seo-keywords.md` | `~/sulla/identity/business/seo-keyword-strategy.md` | Website research, target audience |

---

## Phase 5: Sales Infrastructure

### Human Tasks

| Status | Step | Skill | Output File | Depends On |
|--------|------|-------|-------------|------------|

### Heartbeat Tasks

| Status | Step | Skill | Output File | Depends On |
|--------|------|-------|-------------|------------|
| [ ] | Sales funnel map | `~/sulla/resources/skills/messaging/sales-funnel.md` | `~/sulla/identity/business/sales-funnel.md` | Lead magnet, landing page, email sequence |
| [ ] | Objection handling playbook | `~/sulla/resources/skills/messaging/objection-handling.md` | `~/sulla/identity/business/objection-handling.md` | Empathy maps, brand script |
| [ ] | Pricing strategy | `~/sulla/resources/skills/messaging/pricing-strategy.md` | `~/sulla/identity/business/pricing-strategy.md` | Products/services, target audience |
