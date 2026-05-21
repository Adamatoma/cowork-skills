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

✅ Use `+create` shortcut — simpler flags, always creates under user identity.
❌ `task tasks create --data` may use bot identity and the task won't appear in "My Tasks".

```bash
# ✅ Recommended: use +create shortcut
lark-cli task +create --summary "Task title" --as user

# With due date and assignee
lark-cli task +create \
  --summary "Task title" \
  --description "Details here" \
  --due "2024-01-15" \
  --assignee "ou_xxx" \
  --as user

# Add to a specific task list
lark-cli task +create \
  --summary "Task title" \
  --tasklist-id "tl_xxx" \
  --as user
```

Note: tasks created via API with no tasklist appear in Feishu under "My Tasks → All" — not in any task list view.

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
