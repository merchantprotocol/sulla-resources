# computer

Anthropic native computer use tool — visual screenshot-based interaction with coordinate clicking, typing, and scrolling.

```bash
sulla computer-use/computer '{"action":"screenshot"}'
sulla computer-use/computer '{"action":"click","x":500,"y":300}'
sulla computer-use/computer '{"action":"type","text":"hello world"}'
sulla computer-use/computer '{"action":"scroll","x":500,"y":300,"direction":"down"}'
```

## Parameters

| Param       | Type   | Required | Description                          |
|-------------|--------|----------|--------------------------------------|
| `action`    | string | yes      | `screenshot`, `click`, `type`, `scroll`, `key` |
| `x`         | number | no       | X coordinate (for click/scroll)      |
| `y`         | number | no       | Y coordinate (for click/scroll)      |
| `text`      | string | no       | Text to type                         |
| `direction` | string | no       | Scroll direction (`up`, `down`)      |
