# Messaging & Group Chat (lark-cli im)

## Find user / group IDs (required before any messaging operation)

```bash
# Search for a user by name or email
lark-cli contact +search-user --query "Zhang San"
lark-cli contact +search-user --query "zhangsan@company.com"

# List all groups
lark-cli im +list-chats --format table

# Search for a group
lark-cli im +search-chat --query "project team"
```

## Send messages

⚠️ **Actual supported flags for `+messages-send`** (`--receive-id-type`, `--receive-id`, and `--format` are NOT supported):

```bash
# Send a text message to a user (--user-id takes the open_id, starting with ou_)
lark-cli im +messages-send \
  --user-id "ou_xxx" \
  --text "Hi, can you confirm if 10am tomorrow works for the meeting?"

# Send a message to a group (--chat-id takes the group ID, starting with oc_)
lark-cli im +messages-send \
  --chat-id "oc_xxx" \
  --text "@all Weekly sync tomorrow at 10am — please be on time"

# Send a Markdown message
lark-cli im +messages-send \
  --chat-id "oc_xxx" \
  --markdown "**Notice**\n\nPlease review the latest document"
```

⚠️ **Cross-tenant limitation**: If the recipient is a Feishu Personal Edition user (search results show "Feishu Personal"), you cannot initiate a new conversation via `--user-id` — error code 230038.
Workaround: find an existing P2P chat_id and use `--chat-id` instead:

```bash
# Find an existing P2P chat_id from chat history
lark-cli im +chat-messages-list --user-id "ou_xxx" --format table
# Get the chat_id from the output, then:
lark-cli im +messages-send --chat-id "oc_existing_p2p_id" --text "message content"
```

## Reply to a message

```bash
# Reply to a message (requires message_id)
lark-cli im messages reply --message-id "om_xxx" \
  --msg-type text \
  --content '{"text":"Got it, I will be there on time"}'
```

## View message history

```bash
# View recent messages in a group
lark-cli im messages list --container-id-type chat \
  --container-id "oc_xxx" \
  --format pretty

# View a P2P conversation
lark-cli im +list-messages --chat-id "oc_xxx" --page-size 20
```

## Group management

```bash
# Create a group
lark-cli im chats create --name "Q2 Project Team" --description "Q2 project collaboration group"

# View group members
lark-cli im chat_members get --chat-id "oc_xxx" --format table

# Add a member
lark-cli im chat_members create --chat-id "oc_xxx" \
  --body '{"id_list":["ou_xxx"],"member_id_type":"open_id"}'
```

## Upload / share files

```bash
# Upload a file (get file_key for use in messages)
lark-cli im files create --file-type stream \
  --file-name "report.pdf" \
  --file "@/path/to/report.pdf"

# Send a file message
lark-cli im +messages-send --receive-id-type chat_id \
  --receive-id "oc_xxx" \
  --msg-type file \
  --content '{"file_key":"file_xxx"}'
```

## Common ID types

| ID type | Description | How to get |
|---------|-------------|------------|
| `open_id` | User's unique ID under the current app (starts with `ou_`) | `contact +search-user` |
| `user_id` | User's unique ID within the organization | `contact +search-user` |
| `chat_id` | Group ID (starts with `oc_`) | `im +list-chats` |
| `message_id` | Message ID (starts with `om_`) | message list response |
