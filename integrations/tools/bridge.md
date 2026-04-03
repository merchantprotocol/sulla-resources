# Bridge Tools

Track whether the user is actively present. The agent uses this to decide how to communicate — direct chat when the user is active, notifications or async work when they're away.

## get_human_presence

```bash
sulla bridge/get_human_presence '{}'
```

Returns the current presence state: availability (active/away/idle), what view the user is looking at, and last activity timestamp.

## update_human_presence

```bash
sulla bridge/update_human_presence '{"availability":"active","currentView":"chat"}'
```

| Param          | Type   | Required | Description                              |
|----------------|--------|----------|------------------------------------------|
| `availability` | string | no       | `active`, `away`, or `idle`              |
| `currentView`  | string | no       | What the user is looking at (e.g. `chat`, `editor`, `browser`) |

This is typically updated automatically by the UI. The agent reads it to adjust behavior — for example, using `notify_user` when the user is away instead of sending chat messages they won't see.
