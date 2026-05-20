---
name: lark
description: Skill for operating Feishu (Lark) via the official lark-cli tool. Use this skill whenever the user mentions anything Feishu/Lark-related, including but not limited to: sending messages, reading documents, checking calendar, creating tasks, working with Base/multidimensional tables, sending mail, or getting meeting minutes. Keywords: Feishu, Lark, feishu, lark, message, group chat, document, calendar, task, Base, mail, meeting minutes. Even if the user does not say "Feishu" explicitly but is clearly describing a work collaboration scenario (e.g. "send Zhang San a message about tomorrow's meeting", "summarize the meeting notes"), use this skill.
---

# Feishu (Lark) Operations Skill

Based on the official [lark-cli](https://github.com/larksuite/cli) tool, covering 7 Feishu business domains.

## Step 1: Check environment

**Before every use, verify lark-cli is installed and authenticated:**

```bash
lark-cli auth status
```

- If command not found → see `references/setup.md` for installation guide
- If not logged in → run `lark-cli auth login --recommend` to authorize
- If already logged in → proceed with the task

## Domain quick-reference

Pick the reference doc based on user intent:

| User asks about | Read | Core command |
|----------------|------|-------------|
| Messages, group chat, @mention | `references/messaging.md` | `lark-cli im` |
| Documents, Wiki, cloud files | `references/docs.md` | `lark-cli docs` / `lark-cli wiki` |
| Calendar, meetings, scheduling | `references/calendar.md` | `lark-cli calendar` |
| Tasks, to-dos, reminders | `references/tasks.md` | `lark-cli task` |
| Base, multidimensional tables, spreadsheets | `references/base.md` | `lark-cli base` / `lark-cli sheets` |
| Mail, inbox | `references/mail.md` | `lark-cli mail` |
| Meeting minutes, recordings, AI summaries | `references/meetings.md` | `lark-cli vc` / `lark-cli minutes` |

## General command conventions

```bash
# Output format (default JSON; use pretty or table for human-readable)
lark-cli <domain> <command> --format pretty
lark-cli <domain> <command> --format table

# Auto-paginate (add for list operations)
lark-cli <domain> <command> --page-all

# Dry run (preview without executing)
lark-cli <domain> <command> --dry-run

# Identity (default is user; use bot for app-permission operations)
lark-cli <domain> <command> --as user
lark-cli <domain> <command> --as bot

# Inspect any command's parameter structure
lark-cli schema <domain>.<command>
```

## Common issues

- **Insufficient permissions**: add scopes with `lark-cli auth login --scope "<scope-name>"`
- **Can't find user ID**: search with `lark-cli contact +search-user --query "name or email"`
- **Can't find group ID**: list all groups with `lark-cli im +chat-list`
- **Command error**: check actual supported flags with `lark-cli <domain> <command> --help`

## Self-update protocol (mandatory)

**Whenever any of the following occurs in a session, immediately update the skill files and memory — no need to ask the user:**

### Triggers
1. A lark-cli command failed and a corrected command succeeded → write the correct command into references/
2. A flag doesn't exist, a command doesn't exist, or behavior differs from documentation → mark ❌ and write correct usage
3. A new `missing_scope` error is resolved by authorizing → append that scope to the gap table in setup.md
4. A platform limitation is discovered (e.g. cross-tenant restriction, permission boundary) → write into the relevant references/ file

### How to update

**Update references/ files**: Edit the relevant `.md` file directly, annotating commands with ✅ (verified working) or ❌ (missing/broken), and document correct usage.

**Update memory**: If a project memory exists in the current session, append new findings to the lark skill memory entry.

### Update format
```markdown
- ✅ `lark-cli im +messages-send --chat-id "oc_xxx" --text "content"`  (verified working)
- ❌ `lark-cli im +messages-send --receive-id-type open_id ...`  (flag does not exist)
```

The purpose of this protocol is to let the skill self-correct with each real-world use, accumulating correct paths across sessions instead of repeating the same mistakes.
