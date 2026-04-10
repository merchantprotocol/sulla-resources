---
slug: business-website-research
title: Business Website Research
tags: [skill, onboarding, business, research, website]
triggers: []
category: research
author: sulla-desktop
---

# Business Website Research

This skill is run as a sub-agent during existing business onboarding. It takes a website URL, crawls key pages, and produces structured identity documents about the business.

---

## Input

- **Website URL** — the business's primary website (e.g. `https://example.com`)

---

## Research Process

### 1. Crawl Key Pages

Starting from the provided URL, visit and analyze:

- **Homepage** — overall positioning, value proposition, branding
- **About / Company page** — mission, team, history, story
- **Products / Services page(s)** — what they sell, pricing if visible
- **Contact / Location page** — where they operate, how to reach them
- **Blog / News** (if present, skim recent posts) — current focus, thought leadership topics
- **Footer links** — social media, legal, additional pages

Don't crawl exhaustively. Focus on the pages that reveal who the business is, what it offers, who the owner is and who it serves.

### 2. Extract Key Information

From the pages visited, extract:

**Business Identity:**
- Business name
- Tagline / value proposition
- Industry / vertical
- Location(s)
- Year founded (if mentioned)
- Team size signals (solo, small team, company)

**Products and Services:**
- Each distinct product or service offered
- Descriptions, features, pricing (if public)
- Primary vs. secondary offerings
- Any free tiers, trials, or lead magnets

**Target Audience:**
- Who the website speaks to (language, tone, examples)
- Specific customer segments mentioned
- Industries or verticals served
- Business size targets (SMB, enterprise, consumer)

**Market Positioning:**
- How they differentiate from competitors
- Key messaging themes
- Social proof (testimonials, logos, case studies)
- Trust signals (certifications, partnerships, awards)

**Digital Presence:**
- Social media links found
- Blog/content strategy signals
- SEO/marketing sophistication level
- Tech stack signals (if detectable)

---

## Output Files

Write each document to the business identity directory. If a file already exists, merge new information — don't overwrite.

### 1. Products and Services

Write to `~/sulla/identity/business/products-and-services.md`:

```markdown
---
id: business-products-services
type: business
source: website-research
date: [YYYY-MM-DD]
website: [URL]
---

# Products and Services

## Primary Offerings
[Each product/service with description, features, pricing if known]

## Secondary Offerings
[Supporting products, add-ons, consulting, etc.]

## Pricing
[Pricing model, tiers, public prices if available]

## Free / Lead Generation
[Free tools, trials, lead magnets, content offers]
```

### 2. Target Audience

Write to `~/sulla/identity/business/target-audience.md`:

```markdown
---
id: business-target-audience
type: business
source: website-research
date: [YYYY-MM-DD]
website: [URL]
---

# Target Audience

## Primary Segments
[Who the website is speaking to — specific descriptions]

## Industries Served
[Verticals or industries mentioned]

## Customer Size
[SMB, mid-market, enterprise, consumer]

## Pain Points Addressed
[Problems the business claims to solve]

## Social Proof
[Testimonials, case studies, logos — summarized]
```

### 3. Market Positioning

Write to `~/sulla/identity/business/market-positioning.md`:

```markdown
---
id: business-market-positioning
type: business
source: website-research
date: [YYYY-MM-DD]
website: [URL]
---

# Market Positioning

## Value Proposition
[Core value prop from the homepage / key pages]

## Differentiators
[How they position against alternatives]

## Key Messaging Themes
[Recurring themes in their copy]

## Brand Voice
[Tone, style, personality of the website copy]

## Digital Presence
- Social: [links found]
- Content: [blog frequency, topics]
- Sophistication: [basic / intermediate / advanced]
```

---

## Rules

1. **Be thorough but fast.** Visit the key pages, extract what matters, move on. Don't crawl every link.
2. **Only report what you find.** Don't invent or assume. If pricing isn't public, say so. If the about page is thin, note that.
3. **Mark everything with source.** Every claim should be traceable to a page on the website.
4. **Don't duplicate into the onboarding file.** The parent skill writes `onboarding.md`. This skill writes the research documents.
5. **If the website is down or inaccessible**, report that back to the parent agent — don't silently fail.
