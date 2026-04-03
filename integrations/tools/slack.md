# Slack Tools

Send messages, search users, and manage Slack interactions.

## slack_send_message
```bash
sulla slack/send_message '{"channel":"general","text":"Hello from Sulla"}'
```

## slack_search_users
```bash
sulla slack/search_users '{"query":"jonathon"}'
```

## slack_thread
```bash
sulla slack/thread '{"channel":"C12345","ts":"1234567890.123456"}'
```

## slack_update
```bash
sulla slack/update '{"channel":"C12345","ts":"1234567890.123456","text":"Updated message"}'
```

## slack_unreact
```bash
sulla slack/unreact '{"channel":"C12345","ts":"1234567890.123456","name":"thumbsup"}'
```

## slack_user
```bash
sulla slack/user '{"userId":"U12345"}'
```

## slack_connection_health
```bash
sulla slack/connection_health '{}'
```
