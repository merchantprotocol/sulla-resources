# Calendar Tools

Manage calendar events and reminders.

## calendar_list
```bash
sulla calendar/list '{"startDate":"2026-04-01","endDate":"2026-04-30"}'
```

## calendar_list_upcoming
```bash
sulla calendar/list_upcoming '{"days":7}'
```

## calendar_get
```bash
sulla calendar/get '{"eventId":"abc123"}'
```

## calendar_create
```bash
sulla calendar/create '{"title":"Team standup","startTime":"2026-04-02T09:00:00","endTime":"2026-04-02T09:30:00"}'
```

## calendar_update
```bash
sulla calendar/update '{"eventId":"abc123","title":"Updated title"}'
```

## calendar_cancel
```bash
sulla calendar/cancel '{"eventId":"abc123"}'
```

## calendar_delete
```bash
sulla calendar/delete '{"eventId":"abc123"}'
```
