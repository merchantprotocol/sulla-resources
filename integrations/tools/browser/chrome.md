# Chrome Tools

Browser-level operations — cookies, history, notifications, network, storage.

## notify_user
```bash
sulla chrome/notify_user '{"title":"Task Complete","message":"The build finished successfully"}'
```

## background_browse
```bash
sulla chrome/background_browse '{"url":"https://example.com"}'
```
Browse in a hidden tab without disrupting the user's visible browser.

## search_history
```bash
sulla chrome/search_history '{"query":"github","maxResults":20}'
```

## search_conversations
```bash
sulla chrome/search_conversations '{"query":"twenty CRM setup"}'
```

## manage_cookies
```bash
sulla chrome/manage_cookies '{"action":"get","url":"https://github.com"}'
```

## agent_storage
```bash
sulla chrome/agent_storage '{"action":"get","key":"last_sync"}'
sulla chrome/agent_storage '{"action":"set","key":"last_sync","value":"2026-04-01"}'
```

## monitor_network
```bash
sulla chrome/monitor_network '{"duration":5000}'
```

## schedule_alarm
```bash
sulla chrome/schedule_alarm '{"action":"set","name":"check-deploy","delayMinutes":5}'
```

## modify_history
```bash
sulla chrome/modify_history '{"action":"delete","url":"https://example.com"}'
```
