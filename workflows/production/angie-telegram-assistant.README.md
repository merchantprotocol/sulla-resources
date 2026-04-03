# Angie - Telegram AI Assistant Workflow

## Overview

This workflow recreates the n8n "Angie" Telegram AI assistant in Sulla's workflow system. It serves as a proof of concept for importing/converting n8n workflows.

## Original n8n Workflow

**Source:** n8n template #2462 - "Angie, AI Assistant 👩🏻🏫"  
**Author:** Derek Cheung  
**Video Tutorial:** https://youtu.be/pXjowPc6V2s

### n8n Node Structure

```
Telegram Trigger → AllowList → If1 (security check) → Voice or Text → If (voice detection)
                                                               ↓
                    ┌──────────────────────────────────────────┴──────────────────────────────────────────┐
                    │                                                                                      │
              [Voice Branch]                                                                        [Text Branch]
                    │                                                                                      │
         Get Voice File → Speech to Text                                                          Extract Text
                    │                                                                                      │
                    └──────────────────────────────────────────┬──────────────────────────────────────────┘
                                                               │
                                                               ↓
                                                Angie AI Agent (OpenAI + Memory + Tools)
                                                - Google Calendar tool
                                                - Gmail tool
                                                - Baserow Tasks tool
                                                - Baserow Contacts tool
                                                - Window Buffer Memory
                                                               │
                                                               ↓
                                                          Telegram Response
```

## Sulla Workflow Conversion

### Node Mapping

| n8n Node | Sulla Node | Type | Notes |
|----------|-----------|------|-------|
| Listen for incoming events | node-telegram-trigger | webhook | Telegram webhook trigger |
| AllowList | node-allowlist-check | condition | Security check on chat ID |
| If1 | (merged into allowlist) | condition | Combined security check |
| Voice or Text | node-voice-text-router | condition | Route voice vs text |
| Get Voice File | node-get-voice-file | agent | Telegram file downloader |
| Speech to Text | node-speech-to-text | agent | OpenAI Whisper transcriber |
| (text path) | node-extract-text | agent | Extract message text |
| (merge) | node-merge-input | merge | Combine branches |
| Angie, AI Assistant | node-angie-agent | agent | Main AI assistant with tools |
| Telegram | node-telegram-response | agent | Send response |

### Key Differences

1. **Memory**: n8n uses `@n8n/n8n-nodes-langchain.memoryBufferWindow`; Sulla uses agent-level `memory` config
2. **Tools**: n8n connects tools via `ai_tool` connections; Sulla declares tools in agent config
3. **Integrations**: n8n uses credential nodes; Sulla uses integration configs in `~/sulla/integrations/`
4. **Routing**: n8n uses `If` nodes with conditions; Sulla uses `condition` subtype with `onTrue`/`onFalse`

### Required Integrations

- `telegram` - Bot API for webhook and file downloads
- `openai` - GPT-4o for chat, Whisper for transcription
- `gmail` - Read/filter emails
- `google-calendar` (gcal) - Fetch calendar events
- `baserow` - Tasks and contacts database (not yet in integrations)

### Required Agents

1. `telegram-file-downloader` - Downloads voice files from Telegram
2. `openai-transcriber` - Transcribes audio with Whisper
3. `angie-assistant` - Main assistant with memory and tools

### File Locations

- **Workflow:** `~/sulla/workflows/production/angie-telegram-assistant.yaml`
- **Agents:** `~/sulla/agents/angie-assistant/`, `~/sulla/agents/telegram-file-downloader/`, `~/sulla/agents/openai-transcriber/`
- **Temp files:** `~/sulla/temp/voice-messages/`

## Testing Checklist

- [ ] Telegram webhook receives messages
- [ ] Allowlist security check works
- [ ] Voice messages route to transcription
- [ ] Text messages pass through correctly
- [ ] Angie agent has memory across messages
- [ ] Gmail tool filters promotional emails
- [ ] Calendar tool filters by date relevance
- [ ] Telegram response sends with Markdown formatting

## Notes for n8n Import Automation

This manual conversion reveals patterns that could be automated:

1. **Trigger nodes** → Sulla `webhook` or `schedule` subtypes
2. **Action nodes** → Sulla `agent` subtypes with orchestrator instructions
3. **Condition nodes** → Sulla `condition` subtypes with leftValue/rightValue
4. **Tool connections** → Sulla agent `tools` array
5. **Memory nodes** → Sulla agent `memory` config
6. **Credential references** → Sulla integration configs

