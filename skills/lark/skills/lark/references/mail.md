# Mail (lark-cli mail)

## View mail

```bash
# View inbox (recent messages)
lark-cli mail +inbox --format table

# List mail folders
lark-cli mail user_mailboxes list --format table

# List inbox messages (--page-all to get all)
lark-cli mail user_mailbox_messages list \
  --user-mailbox-id "me" \
  --format table \
  --page-all

# Read a specific message (requires message_id)
lark-cli mail user_mailbox_messages get \
  --user-mailbox-id "me" \
  --message-id "mail_xxx" \
  --format pretty
```

## Search mail

```bash
# Search by subject
lark-cli mail user_mailbox_messages list \
  --user-mailbox-id "me" \
  --params '{"subject": "meeting notes"}' \
  --format table

# Search by sender
lark-cli mail user_mailbox_messages list \
  --user-mailbox-id "me" \
  --params '{"from": "alice@company.com"}' \
  --format table
```

## Send mail

```bash
# Send a new email
lark-cli mail user_mailbox_messages create \
  --user-mailbox-id "me" \
  --body '{
    "subject": "Q2 Project Status Update",
    "to": [{"mail_address": "manager@company.com", "name": "Manager"}],
    "cc": [{"mail_address": "team@company.com"}],
    "body": {"content": "<h2>This Week</h2><p>Completed the initial draft of the requirements doc...</p>", "content_type": "html"}
  }'

# Send a plain text email
lark-cli mail user_mailbox_messages create \
  --user-mailbox-id "me" \
  --body '{
    "subject": "Meeting Reminder for Tomorrow",
    "to": [{"mail_address": "colleague@company.com"}],
    "body": {"content": "Meeting tomorrow at 10am in Room 301, Building A. Please be on time.", "content_type": "plain_text"}
  }'
```

## Reply / forward mail

```bash
# Reply to a message
lark-cli mail user_mailbox_messages reply \
  --user-mailbox-id "me" \
  --message-id "mail_xxx" \
  --body '{
    "body": {"content": "Received, I will be there on time.", "content_type": "plain_text"}
  }'

# Forward a message
lark-cli mail user_mailbox_messages forward \
  --user-mailbox-id "me" \
  --message-id "mail_xxx" \
  --body '{
    "to": [{"mail_address": "other@company.com"}],
    "body": {"content": "Forwarding for your reference", "content_type": "plain_text"}
  }'
```

## Draft management

```bash
# Create a draft
lark-cli mail user_mailbox_drafts create \
  --user-mailbox-id "me" \
  --body '{
    "subject": "Draft Subject",
    "to": [{"mail_address": "recipient@company.com"}],
    "body": {"content": "Email body...", "content_type": "plain_text"}
  }'

# Send a draft
lark-cli mail user_mailbox_drafts send \
  --user-mailbox-id "me" \
  --draft-id "draft_xxx"
```

## Send mail with attachments

```bash
# Upload attachment first to get attachment_token
lark-cli mail user_mailbox_message_attachments create \
  --user-mailbox-id "me" \
  --file "@/path/to/report.pdf" \
  --file-name "report.pdf"

# Send email with attachment
lark-cli mail user_mailbox_messages create \
  --user-mailbox-id "me" \
  --body '{
    "subject": "Report Attached",
    "to": [{"mail_address": "manager@company.com"}],
    "body": {"content": "Please see the attached report.", "content_type": "plain_text"},
    "attachments": [{"attachment_token": "attach_xxx"}]
  }'
```

## Mail flags

```bash
# Mark as read
lark-cli mail user_mailbox_messages patch \
  --user-mailbox-id "me" \
  --message-id "mail_xxx" \
  --body '{"is_read": true}'

# Star a message
lark-cli mail user_mailbox_messages patch \
  --user-mailbox-id "me" \
  --message-id "mail_xxx" \
  --body '{"is_starred": true}'
```
