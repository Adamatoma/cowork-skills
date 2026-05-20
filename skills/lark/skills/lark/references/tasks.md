# Task Management (lark-cli task)

## View tasks

```bash
# View all my tasks
lark-cli task +my-tasks --format table

# View tasks in a specific task list
lark-cli task tasks list \
  --tasklist-guid "tl_xxx" \
  --format table

# View task details
lark-cli task tasks get --task-guid "t_xxx" --format pretty

# View all task lists
lark-cli task tasklists list --format table
```

## Create a task

```bash
# Create a simple task
lark-cli task tasks create \
  --body '{
    "summary": "Complete Q2 requirements document",
    "description": "Initial draft due by end of this Friday",
    "due": {"timestamp": "1700100000"}
  }'

# Create a task and assign it to someone
lark-cli task tasks create \
  --body '{
    "summary": "Review design mockups",
    "due": {"timestamp": "1700100000"},
    "members": [
      {"id": "ou_xxx", "type": "user", "role": "assignee"}
    ]
  }'

# Create a task and add it to a specific task list
lark-cli task tasks create \
  --body '{
    "summary": "New task",
    "tasklists": [{"tasklist_guid": "tl_xxx", "section_guid": "sec_xxx"}]
  }'
```

## Update task status

```bash
# Complete a task
lark-cli task tasks patch \
  --task-guid "t_xxx" \
  --body '{"completed_at": "auto"}'

# Reopen a task (undo completion)
lark-cli task tasks patch \
  --task-guid "t_xxx" \
  --body '{"completed_at": ""}'

# Update task title
lark-cli task tasks patch \
  --task-guid "t_xxx" \
  --body '{"summary": "Updated task title"}'

# Update due date
lark-cli task tasks patch \
  --task-guid "t_xxx" \
  --body '{"due": {"timestamp": "1700200000"}}'
```

## Subtasks

```bash
# Add a subtask to a task
lark-cli task tasks subtasks create \
  --task-guid "t_xxx" \
  --body '{"summary": "Subtask 1"}'

# List subtasks of a task
lark-cli task tasks subtasks list \
  --task-guid "t_xxx" \
  --format table
```

## Add reminders

```bash
# Add a reminder to a task (30 minutes before due)
lark-cli task tasks reminders create \
  --task-guid "t_xxx" \
  --body '{"relative_fire_minute": 30}'
```

## Task list management

```bash
# Create a task list
lark-cli task tasklists create \
  --body '{"name": "This Week'"'"'s Work"}'

# Add a section to a task list
lark-cli task tasklist_sections create \
  --tasklist-guid "tl_xxx" \
  --body '{"name": "In Progress"}'
```

## Task comments

```bash
# View task comments
lark-cli task task_comments list \
  --task-guid "t_xxx" \
  --format pretty

# Add a comment
lark-cli task task_comments create \
  --task-guid "t_xxx" \
  --body '{"content": "Initial research done — results in the linked doc"}'
```
