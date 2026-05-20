# lark skill

基于官方 [lark-cli](https://github.com/larksuite/cli) 工具，通过 Claude Code 操作飞书（Feishu）的全能技能。

## 功能覆盖

| 业务域 | 能力 |
|--------|------|
| 消息 | 发消息、搜索会话、管理群成员 |
| 文档 | 创建/读取/更新/搜索文档和 Wiki |
| 日历 | 查询/创建日程、RSVP、空闲时间查询、会议室预订 |
| 任务 | 创建任务/子任务/提醒、任务列表管理 |
| 多维表格/电子表格 | 查询/编辑多维表格记录和电子表格数据 |
| 邮件 | 收发/回复/转发邮件、草稿管理 |
| 会议纪要 | AI 摘要/待办/转录、上传录音文件 |

## 前置条件

- macOS / Linux
- 飞书账号

## 安装

### 1. 安装 lark-cli

```bash
# macOS arm64（Apple Silicon）
curl -L -o /tmp/lark-cli.tar.gz \
  https://github.com/larksuite/cli/releases/download/v1.0.34/lark-cli-1.0.34-darwin-arm64.tar.gz
tar -xzf /tmp/lark-cli.tar.gz -C /tmp/
mkdir -p ~/bin && cp /tmp/lark-cli ~/bin/lark-cli && chmod +x ~/bin/lark-cli
echo 'export PATH="$HOME/bin:$PATH"' >> ~/.zshrc && export PATH="$HOME/bin:$PATH"

# macOS amd64（Intel）
curl -L -o /tmp/lark-cli.tar.gz \
  https://github.com/larksuite/cli/releases/download/v1.0.34/lark-cli-1.0.34-darwin-amd64.tar.gz
tar -xzf /tmp/lark-cli.tar.gz -C /tmp/
mkdir -p ~/bin && cp /tmp/lark-cli ~/bin/lark-cli && chmod +x ~/bin/lark-cli
```

> ⚠️ 不要用 `npx @larksuite/cli@latest install`，在 Node.js v22+ 会报 `ERR_REQUIRE_ESM` 错误。

### 2. 初始化并一次性完成授权

```bash
# 初始化配置 — 出现提示时选择 feishu.cn
lark-cli config init

# 一次性完整授权（包含本 skill 所需的全部 scope）
lark-cli auth login --no-wait --recommend --scope "im:message.send_as_user search:message"
```

将输出中的 `verification_url` 在浏览器中打开，完成飞书 OAuth 授权后，执行：

```bash
lark-cli auth login --device-code <上一步输出的 device_code>
```

### 3. 验证

```bash
lark-cli auth status
```

## 使用方式

安装后直接用自然语言描述你想做的事：

- "帮我查一下我今天的飞书日历"
- "给'研发群'发消息说明天下午3点开会"
- "读取这个飞书文档：https://xxx.feishu.cn/docx/..."
- "帮我在飞书上创建一个任务"

skill 会自动路由到正确的业务域。

## 已知限制

- **跨租户消息**：无法通过 API 向飞书个人版用户发起新私信（错误码 230038）。解决方法：通过 `lark-cli im +chat-messages-list --user-id <ou_id>` 找到已有私聊的 chat_id，再用 `--chat-id` 发送。
- **Token 过期**：用户 token 约 2 小时后过期，执行 `lark-cli auth login --no-wait --recommend --scope "im:message.send_as_user search:message"` 刷新。
