# 任务管理（lark-cli task）

## 查看任务

```bash
# 查看我的所有任务
lark-cli task +my-tasks --format table

# 查看某个任务列表下的任务
lark-cli task tasks list \
  --tasklist-guid "tl_xxx" \
  --format table

# 查看任务详情
lark-cli task tasks get --task-guid "t_xxx" --format pretty

# 查看所有任务列表
lark-cli task tasklists list --format table
```

## 创建任务

```bash
# 创建简单任务
lark-cli task tasks create \
  --body '{
    "summary": "完成Q2项目需求文档",
    "description": "需要在本周五前完成初稿",
    "due": {"timestamp": "1700100000"}
  }'

# 创建任务并指派给他人
lark-cli task tasks create \
  --body '{
    "summary": "Review 设计稿",
    "due": {"timestamp": "1700100000"},
    "members": [
      {"id": "ou_xxx", "type": "user", "role": "assignee"}
    ]
  }'

# 创建任务并加入指定任务列表
lark-cli task tasks create \
  --body '{
    "summary": "新任务",
    "tasklists": [{"tasklist_guid": "tl_xxx", "section_guid": "sec_xxx"}]
  }'
```

## 更新任务状态

```bash
# 完成任务
lark-cli task tasks patch \
  --task-guid "t_xxx" \
  --body '{"completed_at": "auto"}'

# 重新打开任务（取消完成）
lark-cli task tasks patch \
  --task-guid "t_xxx" \
  --body '{"completed_at": ""}'

# 修改任务标题
lark-cli task tasks patch \
  --task-guid "t_xxx" \
  --body '{"summary": "更新后的任务标题"}'

# 修改截止时间
lark-cli task tasks patch \
  --task-guid "t_xxx" \
  --body '{"due": {"timestamp": "1700200000"}}'
```

## 子任务

```bash
# 为任务添加子任务
lark-cli task tasks subtasks create \
  --task-guid "t_xxx" \
  --body '{"summary": "子任务1"}'

# 查看任务的子任务列表
lark-cli task tasks subtasks list \
  --task-guid "t_xxx" \
  --format table
```

## 添加提醒

```bash
# 为任务添加提醒（截止前 30 分钟）
lark-cli task tasks reminders create \
  --task-guid "t_xxx" \
  --body '{"relative_fire_minute": 30}'
```

## 任务列表管理

```bash
# 创建任务列表
lark-cli task tasklists create \
  --body '{"name": "本周工作"}'

# 在任务列表中添加分组（Section）
lark-cli task tasklist_sections create \
  --tasklist-guid "tl_xxx" \
  --body '{"name": "待处理"}'
```

## 任务评论

```bash
# 查看任务评论
lark-cli task task_comments list \
  --task-guid "t_xxx" \
  --format pretty

# 添加评论
lark-cli task task_comments create \
  --task-guid "t_xxx" \
  --body '{"content": "已完成初步调研，结果见文档"}'
```
