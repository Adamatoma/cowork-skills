# Meeting Minutes & Recordings (lark-cli vc / minutes)

## List meetings

```bash
# List recent meeting recordings
lark-cli vc +list-meetings --format table

# Query meetings by time range
lark-cli vc reserves list \
  --params '{"start_time": "1700000000", "end_time": "1700200000"}' \
  --format table

# Get meeting details (including recording info)
lark-cli vc reserves get \
  --reserve-id "rv_xxx" \
  --format pretty
```

## Get meeting minutes / transcripts

```bash
# Get meeting minutes metadata
lark-cli minutes minutes get \
  --minute-id "obm_xxx" \
  --format pretty

# Get AI-generated summary
lark-cli minutes +ai-summary --minute-id "obm_xxx" --format pretty

# Get AI-extracted to-dos
lark-cli minutes +ai-todos --minute-id "obm_xxx" --format pretty

# Get AI chapter structure
lark-cli minutes +ai-chapters --minute-id "obm_xxx" --format pretty

# Get full transcript text
lark-cli minutes minute_statistics get \
  --minute-id "obm_xxx" \
  --format pretty
```

## Get recording download link

```bash
# View meeting recording (get download link)
lark-cli vc meeting_recordings get \
  --meeting-id "vm_xxx" \
  --format pretty
```

## Upload audio/video to generate minutes

```bash
# Upload a local audio file to generate minutes
lark-cli minutes +upload-audio \
  --file "@/path/to/meeting.mp3" \
  --title "Project Discussion 2024-01-15"

# Upload a video file
lark-cli minutes +upload-video \
  --file "@/path/to/meeting.mp4" \
  --title "Product Review Meeting"
```

## Workflow: meeting minutes aggregation report

```bash
# Use the built-in workflow skill to generate an aggregated meeting notes report
# This workflow automatically finds recent meetings and formats them into a structured report
lark-cli +workflow-meeting-summary
```

## Common scenarios

### Scenario: organize yesterday's meeting notes

```bash
# 1. Find yesterday's meetings
YESTERDAY_START=$(date -v-1d -v0H -v0M -v0S +%s)
YESTERDAY_END=$(date -v-1d -v23H -v59M -v59S +%s)

lark-cli vc reserves list \
  --params "{\"start_time\": \"$YESTERDAY_START\", \"end_time\": \"$YESTERDAY_END\"}" \
  --format table

# 2. Get the meeting ID, then view the minutes
lark-cli minutes minutes get --minute-id "obm_xxx" --format pretty

# 3. Get AI summary
lark-cli minutes +ai-summary --minute-id "obm_xxx"

# 4. Get AI-extracted action items
lark-cli minutes +ai-todos --minute-id "obm_xxx"
```

### Scenario: turn meeting minutes into a Feishu document

```bash
# 1. Get AI summary
SUMMARY=$(lark-cli minutes +ai-summary --minute-id "obm_xxx" --format json)

# 2. Get action items
TODOS=$(lark-cli minutes +ai-todos --minute-id "obm_xxx" --format json)

# 3. Create a document (Claude assembles the content)
lark-cli docs +create \
  --doc-format markdown \
  --content $'<title>Meeting Notes - Weekly Sync</title>\n...'
```

## ID reference

- `minute-id`: starts with `obm_` — found in the meeting minutes URL
- `meeting-id`: starts with `vm_` — found in the video meeting URL or reserve response
- `reserve-id`: starts with `rv_` — returned when a meeting is reserved
- AI features (summary, todos, chapters) require a completed recording with generated minutes
