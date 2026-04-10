---
slug: calendar-automation
title: Calendar System — Events, Triggers, and Time-Based Automation
schemaversion: 1
tags: [calendar, events, schedule, automation, time, trigger, reminder, meeting, appointment]
triggers: ["calendar", "event", "schedule", "meeting", "appointment", "reminder", "time-based", "recurring", "when is", "create event", "upcoming events", "cancel event"]
created_at: 2026-04-01T00:00:00.000Z
updated_at: 2026-04-01T00:00:00.000Z
---

# Calendar System

Source of truth for all time-based actions. Events trigger automatically at the scheduled time with full context injected into the agent.

## Calendar Tools

| Tool | Purpose |
|------|---------|
| `calendar_create` | Create a new event |
| `calendar_list` | List events in a date range |
| `calendar_list_upcoming` | List upcoming events |
| `calendar_get` | Get details of a specific event |
| `calendar_update` | Update an existing event |
| `calendar_delete` | Delete an event |
| `calendar_cancel` | Cancel an event (mark as cancelled) |

## How Events Trigger

When a calendar event's time arrives:
1. The system detects the event
2. Full event context (title, description, attendees, metadata) is injected into the agent's conversation
3. The agent can then act on it — send reminders, run workflows, update systems

## Creating Events

```
calendar_create({
  title: "Weekly standup",
  start: "2026-04-07T09:00:00",
  end: "2026-04-07T09:30:00",
  description: "Team sync — review sprint progress",
  recurrence: "weekly"
})
```

## Patterns

- **Scheduled workflows:** Create a calendar event that triggers a workflow at a specific time
- **Reminders:** Create events that prompt the agent to notify the user
- **Recurring automation:** Combine calendar events with workflows for repeating tasks
