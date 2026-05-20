# lark skill

A Claude Code skill for operating Feishu (Lark) via the official [lark-cli](https://github.com/larksuite/cli) tool.

## Features

Covers 7 business domains:

| Domain | Capabilities |
|--------|-------------|
| Messaging | Send messages, search chats, manage group members |
| Documents | Create, read, update, search docs and Wiki |
| Calendar | Query/create events, RSVP, find free time, meeting rooms |
| Tasks | Create tasks, subtasks, reminders, task lists |
| Base / Sheets | Query/edit multidimensional tables and spreadsheets |
| Mail | Read, send, reply, forward, manage drafts |
| Meeting Minutes | AI summaries, todos, transcripts, upload recordings |

## Prerequisites

- macOS / Linux
- Node.js (for lark-cli installation check)
- A Feishu account

## Installation

### 1. Install lark-cli

```bash
# macOS arm64 (Apple Silicon)
curl -L -o /tmp/lark-cli.tar.gz \
  https://github.com/larksuite/cli/releases/download/v1.0.34/lark-cli-1.0.34-darwin-arm64.tar.gz
tar -xzf /tmp/lark-cli.tar.gz -C /tmp/
mkdir -p ~/bin && cp /tmp/lark-cli ~/bin/lark-cli && chmod +x ~/bin/lark-cli
echo 'export PATH="$HOME/bin:$PATH"' >> ~/.zshrc && export PATH="$HOME/bin:$PATH"

# macOS amd64 (Intel)
curl -L -o /tmp/lark-cli.tar.gz \
  https://github.com/larksuite/cli/releases/download/v1.0.34/lark-cli-1.0.34-darwin-amd64.tar.gz
tar -xzf /tmp/lark-cli.tar.gz -C /tmp/
mkdir -p ~/bin && cp /tmp/lark-cli ~/bin/lark-cli && chmod +x ~/bin/lark-cli
```

### 2. Initialize and authorize (one-time setup)

```bash
# Initialize — select feishu.cn when prompted
lark-cli config init

# One-time full authorization (includes all scopes needed by this skill)
lark-cli auth login --no-wait --recommend --scope "im:message.send_as_user search:message"
```

Open the `verification_url` from the output in your browser, complete the Feishu OAuth flow, then run:

```bash
lark-cli auth login --device-code <device_code_from_above>
```

### 3. Verify

```bash
lark-cli auth status
```

## Usage

Once installed, just describe what you want to do in natural language:

- "帮我查一下我今天的飞书日历"
- "给'研发群'发消息说明天下午3点开会"
- "读取这个飞书文档：https://xxx.feishu.cn/docx/..."
- "帮我在飞书上创建一个任务"

The skill routes to the correct domain automatically.

## Known Limitations

- **Cross-tenant messaging**: Cannot send direct messages to Feishu Personal Edition users via API (error 230038). Workaround: use an existing P2P chat_id found via `lark-cli im +chat-messages-list --user-id <ou_id>`.
- **Token expiry**: User token expires after ~2 hours; refresh with `lark-cli auth login --no-wait --recommend --scope "im:message.send_as_user search:message"`.
