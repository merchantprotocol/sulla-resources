---
slug: sulla-cli
title: How to use the sulla CLI
schemaversion: 1
tags: [sulla, cli, tools, bash, how-to, discovery, browser, screenshot, click]
triggers: ["sulla", "sulla tool", "how do i use", "browse", "browser", "open website", "click", "screenshot", "web page", "fill form", "navigate"]
created_at: 2026-04-20T00:00:00.000Z
updated_at: 2026-04-20T00:00:00.000Z
---

# How to use the `sulla` CLI

Every tool you have is reachable through one command: `sulla`. You always call it from Bash.

## The one shape to remember

```bash
sulla <category>/<tool> '<json-args>'
```

- `<category>` is one of: `meta`, `agents`, `applescript`, `bridge`, `browser`, `calendar`, `docker`, `extensions`, `github`, `kubectl`, `lima`, `n8n`, `notify`, `pg`, `rdctl`, `redis`, `slack`, `vault`.
- `<tool>` is a tool inside that category.
- `<json-args>` is a JSON object for the tool's parameters. Pass `'{}'` when there are no args.

Responses come back as JSON on stdout. Errors come back as JSON too — don't pipe them into `head` or `wc` before you've seen the structure, or you'll lose the payload.

## Discovering tools (don't guess — look)

```bash
sulla --help                    # list all categories
sulla browser --help            # list every tool in `browser` with args
```

If a category name or tool name doesn't match exactly, `sulla --help` is the ground truth. **Do not re-read this skill or search for skill files to discover tools. Run `sulla <category> --help` instead.**

## Worked example — open a web page and read its structure

The task: "open Google Maps, find a business".

One tool call gets you the page open AND a DOM snapshot to plan from — you do **not** need to call snapshot separately after `browser/tab`:

```bash
sulla browser/tab '{"action":"upsert","assetId":"maps","url":"https://www.google.com/maps"}'
```

Response shape (abbreviated):

```
[asset: maps]
# Google Maps
**URL**: https://www.google.com/maps
**Stats**: 3412 tokens | 87 interactive | depth 14

<dehydrated DOM tree with @field-q, @btn-search handles ...>

**How to interact with this page:**
Handle-based (preferred): `browser/click` with @btn-/@link- handles, `browser/fill` with @field- handles.
Pixel-based: `browser/screenshot` to identify coords, then `browser/click_at` / `browser/type_at`.
Escape hatch: `browser/exec` runs arbitrary JS with `window.__sulla` helpers.
```

From there, interact using the handles you got:

```bash
sulla browser/fill '{"assetId":"maps","handle":"@field-q","value":"plumber"}'
sulla browser/press_key '{"assetId":"maps","key":"Enter"}'
sulla browser/snapshot '{"assetId":"maps"}'   # re-read after the results load
```

When you're done:

```bash
sulla browser/tab '{"action":"remove","assetId":"maps"}'
```

## Three habits that save calls

1. **Read the opener's response before making more calls.** `browser/tab` upsert already returns the page snapshot. Don't follow it with `browser/snapshot` unless the page has changed since.
2. **Reuse assetIds.** One `maps` tab per session. Navigate it with another upsert instead of opening a new one.
3. **If a call fails with "tool not found" or "unknown action", run `sulla <category> --help` — the exact tool name and enum values are there.** Don't guess variants.

## Screenshots return a path, not an image

`browser/screenshot` and any tool that auto-captures (click_at, type_at, hover) save the JPEG to disk and return `{ screenshot: { path, width, height, bytes } }`. To see the image, use the `Read` tool on the returned path — it'll load into vision context properly. Never try to base64-decode the path or dump it into the terminal.
