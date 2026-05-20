# 消息与群聊（lark-cli im）

## 查找用户/群组 ID（操作前必须有 ID）

```bash
# 按姓名或邮箱搜索用户
lark-cli contact +search-user --query "张三"
lark-cli contact +search-user --query "zhangsan@company.com"

# 列出所有群组
lark-cli im +list-chats --format table

# 搜索群组
lark-cli im +search-chat --query "项目组"
```

## 发送消息

⚠️ **`+messages-send` 实际可用 flag**（不支持 `--receive-id-type`/`--receive-id`/`--format`）：

```bash
# 发文本消息给用户（--user-id 填 open_id，即 ou_ 开头的 ID）
lark-cli im +messages-send \
  --user-id "ou_xxx" \
  --text "你好，明天上午10点开会，请确认是否方便。"

# 发消息到群组（--chat-id 填 oc_ 开头的群 ID）
lark-cli im +messages-send \
  --chat-id "oc_xxx" \
  --text "@所有人 明天10点项目周会，请准时参加"

# 发 Markdown 消息
lark-cli im +messages-send \
  --chat-id "oc_xxx" \
  --markdown "**通知**\n\n请查看最新文档"
```

⚠️ **跨租户限制**：若对方是飞书个人版用户（搜索结果显示"飞书个人版"），无法通过 `--user-id` 直接发起新会话，错误码 230038。
解决方法：先找到已有的私聊 chat_id，用 `--chat-id` 发送：

```bash
# 通过已有聊天记录找到 P2P chat_id
lark-cli im +chat-messages-list --user-id "ou_xxx" --format table
# 从输出的 chat_id 字段获取，然后：
lark-cli im +messages-send --chat-id "oc_已有私聊ID" --text "消息内容"
```

## 回复消息

```bash
# 回复某条消息（需要 message_id）
lark-cli im messages reply --message-id "om_xxx" \
  --msg-type text \
  --content '{"text":"收到，明天准时参加"}'
```

## 查看消息历史

```bash
# 查看某群最近消息
lark-cli im messages list --container-id-type chat \
  --container-id "oc_xxx" \
  --format pretty

# 查看某用户与自己的私聊
lark-cli im +list-messages --chat-id "oc_xxx" --page-size 20
```

## 群组管理

```bash
# 创建群组
lark-cli im chats create --name "Q2项目组" --description "Q2项目协作群"

# 查看群成员
lark-cli im chat_members get --chat-id "oc_xxx" --format table

# 添加成员
lark-cli im chat_members create --chat-id "oc_xxx" \
  --body '{"id_list":["ou_xxx"],"member_id_type":"open_id"}'
```

## 上传/分享文件

```bash
# 上传文件（获取 file_key 后可在消息中引用）
lark-cli im files create --file-type stream \
  --file-name "report.pdf" \
  --file "@/path/to/report.pdf"

# 发送文件消息
lark-cli im +messages-send --receive-id-type chat_id \
  --receive-id "oc_xxx" \
  --msg-type file \
  --content '{"file_key":"file_xxx"}'
```

## 常用 ID 类型说明

| ID 类型 | 说明 | 获取方式 |
|--------|------|---------|
| `open_id` | 用户在当前 App 下的唯一 ID | `contact +search-user` |
| `user_id` | 用户在企业内的唯一 ID | `contact +search-user` |
| `chat_id` | 群组 ID（oc_ 开头） | `im +list-chats` |
| `message_id` | 消息 ID（om_ 开头） | 消息列表响应 |
