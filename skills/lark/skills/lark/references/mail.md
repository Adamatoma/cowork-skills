# 邮件（lark-cli mail）

## 查看邮件

```bash
# 查看收件箱（最近邮件）
lark-cli mail +inbox --format table

# 列出邮件文件夹
lark-cli mail user_mailboxes list --format table

# 列出收件箱邮件（page-all 获取全部）
lark-cli mail user_mailbox_messages list \
  --user-mailbox-id "me" \
  --format table \
  --page-all

# 读取邮件详情（需要 message_id）
lark-cli mail user_mailbox_messages get \
  --user-mailbox-id "me" \
  --message-id "mail_xxx" \
  --format pretty
```

## 搜索邮件

```bash
# 搜索邮件
lark-cli mail user_mailbox_messages list \
  --user-mailbox-id "me" \
  --params '{"subject": "会议纪要"}' \
  --format table

# 按发件人搜索
lark-cli mail user_mailbox_messages list \
  --user-mailbox-id "me" \
  --params '{"from": "zhangsan@company.com"}' \
  --format table
```

## 发送邮件

```bash
# 发送新邮件
lark-cli mail user_mailbox_messages create \
  --user-mailbox-id "me" \
  --body '{
    "subject": "Q2项目进展汇报",
    "to": [{"mail_address": "manager@company.com", "name": "部门经理"}],
    "cc": [{"mail_address": "team@company.com"}],
    "body": {"content": "<h2>本周进展</h2><p>完成了需求文档的初稿...</p>", "content_type": "html"}
  }'

# 发送纯文本邮件
lark-cli mail user_mailbox_messages create \
  --user-mailbox-id "me" \
  --body '{
    "subject": "明天开会通知",
    "to": [{"mail_address": "colleague@company.com"}],
    "body": {"content": "明天上午10点在A区301会议室开会，请准时参加。", "content_type": "plain_text"}
  }'
```

## 回复/转发邮件

```bash
# 回复邮件
lark-cli mail user_mailbox_messages reply \
  --user-mailbox-id "me" \
  --message-id "mail_xxx" \
  --body '{
    "body": {"content": "收到，我会准时参加。", "content_type": "plain_text"}
  }'

# 转发邮件
lark-cli mail user_mailbox_messages forward \
  --user-mailbox-id "me" \
  --message-id "mail_xxx" \
  --body '{
    "to": [{"mail_address": "other@company.com"}],
    "body": {"content": "转发给你参考", "content_type": "plain_text"}
  }'
```

## 草稿管理

```bash
# 创建草稿
lark-cli mail user_mailbox_drafts create \
  --user-mailbox-id "me" \
  --body '{
    "subject": "草稿标题",
    "to": [{"mail_address": "recipient@company.com"}],
    "body": {"content": "邮件内容...", "content_type": "plain_text"}
  }'

# 发送草稿
lark-cli mail user_mailbox_drafts send \
  --user-mailbox-id "me" \
  --draft-id "draft_xxx"
```

## 带附件发邮件

```bash
# 先上传附件获取 attachment_token
lark-cli mail user_mailbox_message_attachments create \
  --user-mailbox-id "me" \
  --file "@/path/to/report.pdf" \
  --file-name "report.pdf"

# 发送带附件的邮件
lark-cli mail user_mailbox_messages create \
  --user-mailbox-id "me" \
  --body '{
    "subject": "附报告",
    "to": [{"mail_address": "manager@company.com"}],
    "body": {"content": "报告见附件", "content_type": "plain_text"},
    "attachments": [{"attachment_token": "attach_xxx"}]
  }'
```

## 邮件标记

```bash
# 标记为已读
lark-cli mail user_mailbox_messages patch \
  --user-mailbox-id "me" \
  --message-id "mail_xxx" \
  --body '{"is_read": true}'

# 加星标
lark-cli mail user_mailbox_messages patch \
  --user-mailbox-id "me" \
  --message-id "mail_xxx" \
  --body '{"is_starred": true}'
```
