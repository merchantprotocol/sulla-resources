---
slug: browser-automation
title: Browser Automation — Playwright, Chrome APIs, and Decision Framework
schemaversion: 1
tags: [browser, playwright, chrome, automation, dom, tabs, screenshots, cookies, history, notifications]
triggers: ["browse", "browser", "open website", "click", "screenshot", "web page", "navigate", "tab", "cookies", "browsing history", "chrome", "playwright", "scrape", "web scraping", "login", "fill form"]
created_at: 2026-04-01T00:00:00.000Z
updated_at: 2026-04-01T00:00:00.000Z
---

# Browser Automation — Tools, APIs, and Decision Framework

## Tab Hygiene (IMPORTANT)

Before opening a new tab, check if you already have one open with `list_tabs`. Reuse existing tabs by navigating them instead of opening new ones. When you're done with a tab, close it immediately with `browser_tab(action: "remove")`. Never leave tabs open that you're not actively using. If you hit an error on a page, fix the problem on that tab — do not abandon it and open a new one.

Two browser tool systems are available: **Playwright DOM tools** (page interaction) and **Chrome Extension APIs** (browser-level data). Choose based on what you need.

## Decision Tree

| Need | Use |
|------|-----|
| Open a URL, click buttons, fill forms, read page content | Playwright tools |
| Multi-step page interactions (click → wait → read) | `exec_in_page` with `__sulla` helpers |
| Read/search browsing history | Chrome `search_history` |
| Manage cookies | Chrome `manage_cookies` |
| Send desktop notifications | Chrome `notify_user` |
| Store persistent agent data | Chrome `agent_storage` |
| Take a screenshot for visual analysis | Playwright `take_screenshot` |

## Playwright Tools (Page Interaction)

All Playwright tools are called as native tool calls. For the complete tool reference with `exec_in_page`, `__sulla` helpers, and advanced patterns, load the `web-research-playwright` skill.

### Key Tools

| Tool | Purpose |
|------|---------|
| `browser_tab` | Open, navigate, or close tabs |
| `list_tabs` | List all open tabs with assetId, URL, title |
| `get_page_snapshot` | Interactive elements + reader content. Use `mode: "dehydrated"` for compressed DOM |
| `get_page_text` | Clean readable text content |
| `browse_page` | Read content, scroll, search within page |
| `click_element` | Click by element handle from snapshot |
| `set_field` | Fill input fields |
| `press_key` | Keyboard actions (Enter, Tab, etc.) |
| `scroll_to_element` | Scroll to a specific element |
| `wait_for_element` | Wait for element to appear |
| `take_screenshot` | Visual screenshot of the page |
| `exec_in_page` | Execute JavaScript in the page context |
| `synthesize_tabs` | Combine content from multiple tabs |

### Workflow Pattern

1. `browser_tab` → open URL
2. `get_page_snapshot` → see what's on the page
3. `click_element` / `set_field` → interact
4. `get_page_text` → read results

For multi-step flows, use `exec_in_page` with `__sulla.steps()` — everything runs in one round-trip.

## Chrome Extension APIs (Browser-Level)

| Tool | Purpose |
|------|---------|
| `search_history` | Search browsing history by query, date range |
| `modify_history` | Delete history entries |
| `manage_cookies` | Get, set, remove cookies for any domain |
| `notify_user` | Send desktop notifications (use when user isn't watching chat) |
| `agent_storage` | Persistent key-value storage for agent data |
| `monitor_network` | Monitor network requests on active tabs |
| `schedule_alarm` | Schedule timed callbacks |
| `search_conversations` | Search past conversation history |
| `background_browse` | Background tab browsing without UI |

## Important Rules

- Always close tabs when done — open tabs consume resources
- Use `get_page_snapshot` with `mode: "dehydrated"` to save tokens (~5k vs ~50k)
- For login flows, use `vault_autofill` to fill credentials from the vault
- Never hardcode credentials — always use the vault
- If a site blocks automation (CAPTCHA, anti-bot), try a different approach or report to the user
