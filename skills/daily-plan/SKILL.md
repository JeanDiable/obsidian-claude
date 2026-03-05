---
name: daily-plan
description: Generate daily diary with yesterday's review, today's tasks, and diet plan. Integrates TickTick MCP.
---

# Daily Plan

Morning ritual: review yesterday, plan today, generate diary note.

## Vault Path
~/Library/Mobile Documents/iCloud~md~obsidian/Documents/My_note

## Workflow

### Step 1: Gather Context

1. **Read current plans**:
   - Find the current month plan in `70_Plans/` (e.g., `2026计划/202603/`)
   - Find the current week plan
   - Read the diet/fitness plan if one exists

2. **Read yesterday's diary**:
   - Read `10_Diary/YYYY-MM-DD.md` (yesterday's date)
   - Extract: what tasks were planned, what was accomplished, any notes

3. **Query TickTick**:
   - Use TickTick/Dida365 MCP to get yesterday's task completion status
   - Get today's existing tasks if any
   - Get overdue tasks

### Step 2: Generate Today's Diary

Create `10_Diary/YYYY-MM-DD.md`:

```markdown
---
title: "YYYY-MM-DD 日记"
created: YYYY-MM-DD
modified: YYYY-MM-DD
tags: [diary, daily]
description: "Daily plan and review for YYYY-MM-DD"
---

# YYYY-MM-DD

## 昨日回顾
### 任务完成情况
- [x] Completed task 1
- [ ] Incomplete task 2 → [carried over / dropped / rescheduled]

### 复盘
[Brief retrospective: what went well, what didn't, lessons learned]

## 今日计划
### 核心任务 (Must Do)
- [ ] Task from weekly plan
- [ ] Carried over task

### 次要任务 (Should Do)
- [ ] Task 2
- [ ] Task 3

### 可选任务 (Could Do)
- [ ] Task 4

## 饮食计划
[Based on diet plan: meals, macros, calories for today]

## 备注
[Any additional notes, reminders, or thoughts]
```

### Step 3: Sync to TickTick (Optional)

If user confirms, create today's tasks in TickTick via MCP.

## Important Rules
- Run this skill every morning
- Keep the review honest — don't sugarcoat incomplete tasks
- Prioritize tasks using Eisenhower matrix (urgent/important)
- Diet plan should be specific (meals, not just macros)
- If no weekly plan exists for the current week, note it and suggest creating one
- Link to relevant project files where tasks come from
