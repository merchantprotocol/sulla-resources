---
slug: response-formatting
title: Response Formatting — HTML, Shadow DOM, CSS Variables, and Markdown
schemaversion: 1
tags: [formatting, html, css, shadow-dom, markdown, visual, dashboard, chart, table, design, noir, dark-mode]
triggers: ["format response", "html response", "visual", "dashboard", "chart", "table", "css variables", "design system", "dark mode", "formatting", "widget", "rich content", "styled response"]
created_at: 2026-04-01T00:00:00.000Z
updated_at: 2026-04-01T00:00:00.000Z
---

# Response Formatting

## When to Use What

| Content type | Format |
|-------------|--------|
| Simple text, explanations, code | Markdown |
| Dashboards, charts, tables, widgets, visual content | HTML with `<html>` wrapper |
| Background alerts when user isn't watching | `notify_user` via Tools API |

## HTML Responses (Visual Content)

Wrap your entire response in `<html>...</html>` tags. The chat UI renders it in an isolated Shadow DOM with the **Noir Terminal Editorial** design system pre-loaded.

```html
<html>
<div class="card">
  <h2>Dashboard Title</h2>
  <table>
    <tr><th>Metric</th><th>Value</th></tr>
    <tr><td>Revenue</td><td style="color: var(--green)">$12,450</td></tr>
  </table>
</div>
</html>
```

## CSS Variables

Use these — never hardcode colors:

**Backgrounds:**
- `--bg` — page background
- `--surface-1` — card/container background
- `--surface-2` — nested surface
- `--surface-3` — deepest nesting level

**Text:**
- `--text` — primary text
- `--text-muted` — secondary text
- `--text-dim` — tertiary/faded text

**Accents:**
- `--green` — primary accent
- `--green-bright` — highlighted accent
- `--green-glow` — glowing effect

**Borders:**
- `--border` — standard border
- `--border-muted` — subtle border

**Status:**
- `--info` — informational
- `--success` — success state
- `--warning` — warning state
- `--danger` — error/danger state

## Fonts

- `var(--font-display)` — Playfair Display (headlines, titles)
- `var(--font-mono)` — JetBrains Mono (body text, code)
- `var(--font-body)` — System sans-serif (long-form text)

## Aesthetic

Dark mode only. Green-on-black. Noir cinematic feel. Think terminal meets editorial design.

## Markdown Responses

For simple text responses, use standard markdown. No special formatting needed.
