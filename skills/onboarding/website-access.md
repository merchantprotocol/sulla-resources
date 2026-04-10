---
slug: website-access
title: Website Access & Blog Publishing Setup
tags: [skill, onboarding, business, website, cms, blog, human]
triggers: ["connect website", "website credentials", "blog access", "cms setup"]
category: onboarding
author: sulla-desktop
---

# Website Access & Blog Publishing Setup

This is a **human task** — it requires conversation with the user via Sulla Desktop. The heartbeat agent should NOT attempt this skill; it should flag it as `HUMAN ACTION REQUIRED` in ACTIVE_PROJECTS.md.

---

## Prerequisites

- `~/sulla/identity/business/onboarding.md` must exist (contains the website URL)

---

## Conversation Flow

### Step 1: Identify the CMS

Read `~/sulla/identity/business/onboarding.md` for the website URL. Then ask:

> "To start publishing content for your business, I need access to your website. What CMS are you using — WordPress, Ghost, Webflow, Squarespace, or something else?"

If they don't know, help them figure it out based on the URL (check for `/wp-admin`, common CMS patterns, etc.).

---

### Step 2: Collect Credentials

> "I'll need login credentials so I can publish blog posts on your behalf. Can you provide your admin username and password? I'll store them securely in the password vault."

Collect:
- CMS type (WordPress, Ghost, etc.)
- Admin URL (e.g. `https://example.com/wp-admin`)
- Username
- Password or API key
- Any 2FA setup they need to be aware of

Store credentials in the password vault using the vault tool.

---

### Step 3: Test Authentication

Attempt to log in using the stored credentials.

- **Success** → proceed to Step 4
- **Failure** → tell the user what went wrong, ask them to verify credentials

---

### Step 4: Test Draft Blog Post

Create a test blog post:
- Title: "Sulla Test Post — Please Ignore"
- Content: "This is an automated test post to verify publishing access. It will be deleted shortly."
- Status: **Draft** (do not publish yet)

Confirm the draft was created successfully.

---

### Step 5: Test Publishing

Publish the draft post. Confirm it went live.

---

### Step 6: Verify on Live Site

Visit the live URL and confirm the post is visible on the actual website.

- **Visible** → proceed to Step 7
- **Not visible** → troubleshoot (caching, CDN, theme issues), retry

---

### Step 7: Clean Up

Delete the test post (or revert to draft and delete).

---

### Step 8: Write Output Files

#### Website Access Document

Write to `~/sulla/identity/business/website-access.md`:

```markdown
---
id: website-access
type: business
source: onboarding-human-task
date: [YYYY-MM-DD]
updated: [YYYY-MM-DD]
---

# Website Access

- **URL:** [website URL]
- **CMS:** [WordPress/Ghost/etc.]
- **Admin URL:** [admin panel URL]
- **Credentials:** Stored in vault as [vault entry name]
- **Capabilities confirmed:**
  - [x] Authentication
  - [x] Create draft post
  - [x] Publish post
  - [x] Post visible on live site
  - [x] Delete post
```

#### Blog Publishing Verified Document

Write to `~/sulla/identity/business/blog-publishing-verified.md`:

```markdown
---
id: blog-publishing-verified
type: business
source: onboarding-human-task
date: [YYYY-MM-DD]
---

# Blog Publishing Verified

- **Test date:** [YYYY-MM-DD]
- **CMS:** [type]
- **Test post created:** Yes
- **Test post published:** Yes
- **Test post visible on live site:** Yes
- **Test post cleaned up:** Yes
- **Status:** VERIFIED — ready for automated blog publishing
```

---

## After Writing

Update the playbook at `~/sulla/projects/onboarding/playbook.md`:
- Mark "Connect website to vault" as `[x] (YYYY-MM-DD)`
- Mark "Test blog publishing" as `[x] (YYYY-MM-DD)`

---

## Rules

1. **This is a human task.** The heartbeat should never attempt it — only flag it.
2. **Store credentials in the vault.** Never write passwords to markdown files.
3. **Test the full cycle.** Don't mark complete until you've verified a post is visible on the live site.
4. **Clean up after testing.** Delete the test post so it doesn't appear on their actual site.
5. **Both output files required.** `website-access.md` records the setup, `blog-publishing-verified.md` confirms the test passed. Downstream tasks depend on these files.
