---
name: lark
description: 操作飞书（Feishu）的全能技能，基于官方 lark-cli 工具。当用户提到飞书相关操作时必须使用本技能，包括但不限于：发飞书消息、查飞书文档、看飞书日历、创建飞书任务、操作多维表格/Base、收发飞书邮件、获取会议纪要。关键词：飞书、lark、feishu、发消息、群聊、文档、日历、任务、多维表格、邮件、会议纪要。即使用户没有说"飞书"，但明显是在描述工作协作场景（如"发给张三说明天开会"、"把会议记录整理一下"），也应使用本技能。
---

# 飞书（Feishu）操作技能

基于官方 [lark-cli](https://github.com/larksuite/cli) 工具，覆盖飞书 7 大业务域。

## 第一步：检查环境

**每次使用前先检查 lark-cli 是否已安装且完成认证：**

```bash
lark-cli auth status
```

- 若命令不存在 → 参考 `references/setup.md` 引导安装
- 若返回未登录 → 执行 `lark-cli auth login --recommend` 完成授权
- 若返回已登录 → 直接执行任务

## 业务域速查

根据用户意图，选择对应参考文档：

| 用户说的 | 读取文档 | 核心命令 |
|---------|---------|---------|
| 发消息、群聊、@人 | `references/messaging.md` | `lark-cli im` |
| 文档、Wiki、云文件 | `references/docs.md` | `lark-cli docs` / `lark-cli wiki` |
| 日历、会议、约人 | `references/calendar.md` | `lark-cli calendar` |
| 任务、待办、提醒 | `references/tasks.md` | `lark-cli task` |
| 多维表格、Base、表单 | `references/base.md` | `lark-cli base` / `lark-cli sheets` |
| 邮件、收件箱 | `references/mail.md` | `lark-cli mail` |
| 会议纪要、录音、摘要 | `references/meetings.md` | `lark-cli vc` / `lark-cli minutes` |

## 通用命令约定

```bash
# 输出格式（默认 JSON，人类可读用 pretty）
lark-cli <domain> <command> --format pretty
lark-cli <domain> <command> --format table

# 自动翻页（列表类操作加上）
lark-cli <domain> <command> --page-all

# 预览不执行
lark-cli <domain> <command> --dry-run

# 身份切换（默认用户身份，bot 需要 app 权限）
lark-cli <domain> <command> --as user
lark-cli <domain> <command> --as bot

# 查看任意命令的参数结构
lark-cli schema <domain>.<command>
```

## 常见问题

- **权限不足**：用 `lark-cli auth login --scope "<scope名>"` 补充授权
- **找不到用户 ID**：用 `lark-cli contact +search-user --query "姓名或邮箱"` 查找
- **找不到群组 ID**：用 `lark-cli im +chat-list` 列出所有群
- **命令报错**：先用 `lark-cli <domain> <command> --help` 查看实际支持的 flag

## 自我更新协议（必须遵守）

**每次会话中，只要出现以下任一情况，必须立即更新 skill 文件和 memory，不需要询问用户：**

### 触发条件
1. 执行了一条 lark-cli 命令，报错后修正为另一条命令成功 → 将正确命令写入 references/
2. 发现某个 flag 不存在、命令不存在、或行为与文档描述不符 → 标注 ❌ 并写入正确用法
3. 遇到新的 `missing_scope` 错误并授权通过 → 将该 scope 追加到 setup.md 的缺口表
4. 发现某个平台限制（如跨租户、权限边界）→ 写入对应 references/ 文件

### 更新方式

**更新 references/ 文件**：直接 Edit 对应的 `.md` 文件，在相关命令旁标注 ✅（验证可用）或 ❌（不存在/报错），并写明正确用法。

**更新 memory**：若当前会话有 project memory，将新发现追加到 lark skill 相关的 memory 条目中。

### 更新格式
```markdown
- ✅ `lark-cli im +messages-send --chat-id "oc_xxx" --text "内容"`  （验证可用）
- ❌ `lark-cli im +messages-send --receive-id-type open_id ...`  （flag 不存在）
```

这个机制的目的是：让 skill 随着每次真实使用不断自我校正，跨会话积累正确路径，而不是每次都重新试错。
