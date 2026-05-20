# 日历与会议（lark-cli calendar）

## 查看日程

```bash
# 查看今天/本周日程（最常用）
lark-cli calendar +agenda

# 查看指定日期范围的日程（时间戳格式）
lark-cli calendar events instance_view \
  --params '{"calendar_id":"primary","start_time":"1700000000","end_time":"1700086400"}' \
  --format pretty

# 列出日历列表
lark-cli calendar calendars list --format table
```

## 时间戳转换

```bash
# 查看明天的日程（用 date 命令生成时间戳）
START=$(date -v+1d -v0H -v0M -v0S +%s)
END=$(date -v+2d -v0H -v0M -v0S +%s)
lark-cli calendar events instance_view \
  --params "{\"calendar_id\":\"primary\",\"start_time\":\"$START\",\"end_time\":\"$END\"}" \
  --format pretty
```

## 创建日程

```bash
# 创建会议日程
lark-cli calendar events create \
  --calendar-id primary \
  --body '{
    "summary": "Q2项目周会",
    "description": "讨论Q2项目进展",
    "start_time": {"timestamp": "1700100000", "timezone": "Asia/Shanghai"},
    "end_time": {"timestamp": "1700103600", "timezone": "Asia/Shanghai"},
    "attendees": [
      {"type": "user", "user_id": "ou_xxx"},
      {"type": "user", "user_id": "ou_yyy"}
    ],
    "location": {"name": "A区会议室301"}
  }'

# 创建全天日程
lark-cli calendar events create \
  --calendar-id primary \
  --body '{
    "summary": "年假",
    "start_time": {"date": "2024-01-15"},
    "end_time": {"date": "2024-01-16"}
  }'
```

## 更新和删除日程

```bash
# 更新日程（需要 event_id）
lark-cli calendar events patch \
  --calendar-id primary \
  --event-id "evt_xxx" \
  --body '{"summary": "更新后的会议名称"}'

# 删除日程
lark-cli calendar events delete \
  --calendar-id primary \
  --event-id "evt_xxx"
```

## 回复会议邀请（RSVP）

```bash
# 接受
lark-cli calendar +rsvp --event-id "evt_xxx" --status accept

# 拒绝
lark-cli calendar +rsvp --event-id "evt_xxx" --status decline

# 暂定
lark-cli calendar +rsvp --event-id "evt_xxx" --status tentative
```

## 查询空闲时间

```bash
# 查询多人的空闲时间段（便于约会议）
lark-cli calendar freebusy list \
  --body '{
    "time_min": "1700100000",
    "time_max": "1700200000",
    "user_ids": ["ou_xxx", "ou_yyy"]
  }' \
  --format pretty
```

## 会议室

```bash
# 搜索可用会议室
lark-cli calendar +find-room \
  --start-time "1700100000" \
  --end-time "1700103600" \
  --floor "3" \
  --format table

# 列出所有会议室
lark-cli calendar room_levels list --format table
```

## 查看他人日历（需要对方授权）

```bash
# 订阅他人日历
lark-cli calendar calendars subscribe --calendar-id "user_xxx@feishu.cn"

# 查看订阅的日历列表
lark-cli calendar calendars list --format table
```
