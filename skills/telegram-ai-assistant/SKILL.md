---
schemaversion: 1
slug: telegram-ai-assistant
title: "Telegram AI Assistant (Angie)"
section: "Standard Operating Procedures"
category: "Communication & Automation"
tags:
  - skill
  - telegram
  - ai-assistant
  - voice
  - email
  - calendar
  - tasks
order: 1
locked: false
author: seed
---

# Telegram AI Assistant (Angie)

**Triggers**: Human says "telegram assistant", "angie", "voice assistant", "telegram bot", "manage my emails", "check my calendar", "create a task", or sends a message via Telegram.

Angie is your personal AI assistant accessible via Telegram that handles both voice and text messages to manage your digital life - emails, calendar, tasks, and contacts.

---

## Overview

This skill activates the Telegram AI Assistant workflow which:
1. Listens for incoming Telegram messages (text or voice)
2. Verifies the sender is authorized (allowlist)
3. Transcribes voice messages to text (if needed)
4. Processes requests using AI with access to Gmail, Google Calendar, and Baserow (tasks/contacts)
5. Sends responses back via Telegram

---

## Prerequisites

### Required Integrations

| Integration | Purpose | Auth Type |
|-------------|---------|-----------|
| `telegram` | Send/receive messages | Bot token |
| `openai` or `whisper` | Chat model & speech-to-text | API key (bearer) |
| `gmail` | Fetch and read emails | OAuth2 |
| `gcal` | View/manage calendar events | OAuth2 |
| `baserow` | Tasks and contacts database | API key |

### Setup Steps

1. **Configure Telegram Bot**
   - Create a bot via @BotFather on Telegram
   - Get the bot token
   - Add to vault: `vault/write_secrets` with integrationId: "telegram"

2. **Set up Allowlist**
   - Get your Telegram chat ID (send a message to the bot and check logs)
   - Edit the workflow file: `~/sulla/workflows/draft/telegram-ai-assistant.yaml`
   - Replace `<chat_id_1>`, `<chat_id_2>` with authorized IDs in the `node-allowlist-check` node

3. **Connect Gmail & Google Calendar**
   - Ensure OAuth2 credentials are configured
   - Add to vault with integrationIds: "gmail", "gcal"

4. **Set up Baserow Tables**
   - Create a Baserow database with two tables:
     - **Tasks** (tableId: 372174, databaseId: 146496)
     - **Contacts** (tableId: 372177, databaseId: 146496)
   - Add Baserow API token to vault: integrationId: "baserow"

5. **Configure OpenAI/Whisper**
   - Add API key to vault: integrationId: "openai" or "whisper"

---

## Tool Mapping

| Step | Tool | Notes |
|------|------|-------|
| Check authorization | Conditional node | JavaScript check against allowlist |
| Get voice file | `exec` via Telegram API | `/getFile` endpoint |
| Transcribe voice | `exec` via Whisper API | POST to `/v1/audio/transcriptions` |
| Fetch emails | `exec` via Gmail API | Filter by INBOX, UNREAD, date |
| Fetch calendar | `exec` via Google Calendar API | Fields: summary, start(dateTime) |
| Query tasks | `exec` via Baserow API | Table: tasks (372174) |
| Query contacts | `exec` via Baserow API | Table: contacts (372177) |
| AI reasoning | OpenAI Chat Model | With window buffer memory |
| Send response | `exec` via Telegram API | `/sendMessage` endpoint |

---

## Usage Examples

### Voice Commands (send as voice message)
- "What emails do I have today?"
- "Show me my calendar for tomorrow"
- "Create a new task to call John next week"
- "What's Sarah's email address?"

### Text Commands
- "What's on my schedule this week?"
- "Any unread emails from my boss?"
- "Add a reminder to submit the report Friday"
- "Find Mike's phone number"

---

## Workflow Execution

To activate the assistant:

1. **Manual trigger**: User sends a message to the Telegram bot
2. **Webhook**: Telegram POSTs to `/telegram/webhook`
3. **Workflow**: `workflow-telegram-ai-assistant` processes the message

### Trigger Message Format
```json
{
  "message": {
    "from": { "id": "123456789" },
    "text": "What's on my calendar today?",
    // OR for voice:
    "voice": { "file_id": "AgACAgQAAxkBAAICGm..." }
  }
}
```

---

## AI Agent Guidelines

The AI assistant (Angie) follows these rules:

### Email Handling
- Filter out promotional emails automatically
- Summary format: **Sender** | **Date** | **Subject** | Brief summary
- Default date: today (if not specified)

### Calendar Handling
- Only show events relevant to the query
- Don't mention events more than 1 week in the future (unless asked)
- Filter by date range based on user's question

### Task Management
- Use Baserow Tasks table for all task operations
- Support: create, list, update, complete tasks

### Contact Lookup
- Use Baserow Contacts table
- Return: name, email, phone, any other stored info

### Conversation Memory
- Window buffer: last 10 messages per user
- Session key: Telegram chat ID
- Enables contextual follow-up questions

---

## Directory Structure

```
~/sulla/
├── workflows/draft/
│   └── telegram-ai-assistant.yaml    # Main workflow definition
├── skills/telegram-ai-assistant/
│   └── SKILL.md                       # This file
├── integrations/
│   ├── telegram/                      # Telegram bot config
│   ├── openai/                        # Chat & Whisper
│   ├── gmail/                         # Email access
│   ├── gcal/                          # Calendar access
│   └── baserow/                       # Tasks & Contacts
└── daily-logs/
    └── {date}/
        └── agent/
            ├── observations/
            │   └── telegram-assistant.md
            └── thinking/
                └── telegram-assistant.md
```

---

## Troubleshooting

| Error | Fix |
|-------|-----|
| `UNAUTHENTICATED` | Check vault credentials for the relevant integration |
| `Unauthorized user` | Add chat ID to allowlist in workflow |
| `Voice transcription failed` | Verify Whisper/OpenAI API key is valid |
| `No emails found` | Check Gmail OAuth token hasn't expired |
| `Calendar fetch failed` | Re-authenticate Google Calendar OAuth |
| `Baserow query error` | Verify table IDs and database ID match your Baserow setup |
| `Telegram message not sent` | Check bot token and that bot is not blocked by user |

---

## Security Notes

- **Allowlist is critical**: Only authorized chat IDs can use the assistant
- **Credentials in vault**: Never hardcode API keys or tokens
- **OAuth2 tokens**: Gmail and Calendar use OAuth2 - tokens expire and need refresh
- **Session isolation**: Each user's conversation memory is isolated by chat ID

---

## Future Enhancements

- [ ] Add support for creating calendar events
- [ ] Add support for sending emails
- [ ] Add support for voice responses (TTS)
- [ ] Add multi-language support
- [ ] Add custom commands/actions
- [ ] Add analytics and usage tracking
- [ ] Add conversation export feature
