# Calendar & Meetings (lark-cli calendar)

## View events

```bash
# View today's / this week's agenda (most common)
lark-cli calendar +agenda

# View events in a specific date range (Unix timestamp format)
lark-cli calendar events instance_view \
  --params '{"calendar_id":"primary","start_time":"1700000000","end_time":"1700086400"}' \
  --format pretty

# List all calendars
lark-cli calendar calendars list --format table
```

## Timestamp helpers

```bash
# View tomorrow's events (use `date` to generate timestamps)
START=$(date -v+1d -v0H -v0M -v0S +%s)
END=$(date -v+2d -v0H -v0M -v0S +%s)
lark-cli calendar events instance_view \
  --params "{\"calendar_id\":\"primary\",\"start_time\":\"$START\",\"end_time\":\"$END\"}" \
  --format pretty
```

## Create an event

```bash
# Create a meeting event
lark-cli calendar events create \
  --calendar-id primary \
  --body '{
    "summary": "Q2 Weekly Sync",
    "description": "Discuss Q2 project progress",
    "start_time": {"timestamp": "1700100000", "timezone": "Asia/Shanghai"},
    "end_time": {"timestamp": "1700103600", "timezone": "Asia/Shanghai"},
    "attendees": [
      {"type": "user", "user_id": "ou_xxx"},
      {"type": "user", "user_id": "ou_yyy"}
    ],
    "location": {"name": "Meeting Room 301, Building A"}
  }'

# Create an all-day event
lark-cli calendar events create \
  --calendar-id primary \
  --body '{
    "summary": "Annual Leave",
    "start_time": {"date": "2024-01-15"},
    "end_time": {"date": "2024-01-16"}
  }'
```

## Update and delete events

```bash
# Update an event (requires event_id)
lark-cli calendar events patch \
  --calendar-id primary \
  --event-id "evt_xxx" \
  --body '{"summary": "Updated Meeting Title"}'

# Delete an event
lark-cli calendar events delete \
  --calendar-id primary \
  --event-id "evt_xxx"
```

## RSVP to meeting invitations

```bash
# Accept
lark-cli calendar +rsvp --event-id "evt_xxx" --status accept

# Decline
lark-cli calendar +rsvp --event-id "evt_xxx" --status decline

# Tentative
lark-cli calendar +rsvp --event-id "evt_xxx" --status tentative
```

## Find free time

```bash
# Check availability for multiple people (useful for scheduling meetings)
lark-cli calendar freebusy list \
  --body '{
    "time_min": "1700100000",
    "time_max": "1700200000",
    "user_ids": ["ou_xxx", "ou_yyy"]
  }' \
  --format pretty
```

## Meeting rooms

```bash
# Search for available meeting rooms
lark-cli calendar +find-room \
  --start-time "1700100000" \
  --end-time "1700103600" \
  --floor "3" \
  --format table

# List all meeting rooms
lark-cli calendar room_levels list --format table
```

## View others' calendars (requires their authorization)

```bash
# Subscribe to someone's calendar
lark-cli calendar calendars subscribe --calendar-id "user_xxx@feishu.cn"

# List subscribed calendars
lark-cli calendar calendars list --format table
```
