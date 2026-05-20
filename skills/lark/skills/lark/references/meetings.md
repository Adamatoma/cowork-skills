# 会议纪要与会议录音（lark-cli vc / minutes）

## 获取会议纪要

```bash
# 列出最近的会议录制
lark-cli vc +list-meetings --format table

# 按时间范围查询会议
lark-cli vc reserves list \
  --params '{"start_time": "1700000000", "end_time": "1700200000"}' \
  --format table

# 获取会议详情（包含录制信息）
lark-cli vc reserves get \
  --reserve-id "rv_xxx" \
  --format pretty
```

## 获取会议录音/纪要

```bash
# 获取会议纪要元数据
lark-cli minutes minutes get \
  --minute-id "obm_xxx" \
  --format pretty

# 获取纪要 AI 摘要
lark-cli minutes +ai-summary --minute-id "obm_xxx" --format pretty

# 获取纪要 AI 待办事项
lark-cli minutes +ai-todos --minute-id "obm_xxx" --format pretty

# 获取纪要章节结构
lark-cli minutes +ai-chapters --minute-id "obm_xxx" --format pretty

# 获取完整转录文本
lark-cli minutes minute_statistics get \
  --minute-id "obm_xxx" \
  --format pretty
```

## 获取会议录制下载链接

```bash
# 查看会议录制（获取下载链接）
lark-cli vc meeting_recordings get \
  --meeting-id "vm_xxx" \
  --format pretty
```

## 上传音频/视频生成纪要

```bash
# 上传本地录音文件生成纪要
lark-cli minutes +upload-audio \
  --file "@/path/to/meeting.mp3" \
  --title "项目讨论会议 2024-01-15"

# 上传视频文件
lark-cli minutes +upload-video \
  --file "@/path/to/meeting.mp4" \
  --title "产品评审会议"
```

## 工作流：会议纪要聚合报告

```bash
# 使用内置工作流 skill 生成会议纪要聚合报告
# 这个工作流会自动找到最近的会议，整理成结构化报告
lark-cli +workflow-meeting-summary
```

## 常用场景示例

### 场景：整理昨天的会议纪要

```bash
# 1. 找到昨天的会议
YESTERDAY_START=$(date -v-1d -v0H -v0M -v0S +%s)
YESTERDAY_END=$(date -v-1d -v23H -v59M -v59S +%s)

lark-cli vc reserves list \
  --params "{\"start_time\": \"$YESTERDAY_START\", \"end_time\": \"$YESTERDAY_END\"}" \
  --format table

# 2. 获取会议 ID，然后查看纪要
lark-cli minutes minutes get --minute-id "obm_xxx" --format pretty

# 3. 获取 AI 摘要
lark-cli minutes +ai-summary --minute-id "obm_xxx"

# 4. 获取 AI 提取的待办项
lark-cli minutes +ai-todos --minute-id "obm_xxx"
```

### 场景：把会议纪要整理成飞书文档

```bash
# 1. 获取 AI 摘要
SUMMARY=$(lark-cli minutes +ai-summary --minute-id "obm_xxx" --format json)

# 2. 获取待办事项
TODOS=$(lark-cli minutes +ai-todos --minute-id "obm_xxx" --format json)

# 3. 创建文档（Claude 负责整合内容）
lark-cli docs +create \
  --doc-format markdown \
  --content $'<title>会议纪要 - 项目周会</title>\n...'
```

## 说明

- `minute-id`：以 `obm_` 开头，在会议纪要 URL 中可以找到
- `meeting-id`：以 `vm_` 开头，在视频会议 URL 或 reserve 响应中获取
- `reserve-id`：以 `rv_` 开头，预定会议时返回
- AI 功能（摘要、待办、章节）需要会议有完整录制且已生成纪要
