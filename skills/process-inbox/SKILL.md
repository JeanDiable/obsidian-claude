---
name: process-inbox
description: Route inbox notes to appropriate PARA folders with metadata and wikilinks.
---

# Process Inbox

Move notes from 00_Inbox/ to their proper location in PARA, adding metadata and connections.

## Vault Path
~/Library/Mobile Documents/iCloud~md~obsidian/Documents/My_note

## Workflow

### Step 1: List Inbox Contents

1. List all .md files in `00_Inbox/` (including subdirectories)
2. For each file, read content and determine:
   - Best destination folder in PARA:
     - 20_Projects/ — if it's a project, initiative, or actionable plan
     - 30_Areas/A1-A4 — if it relates to an ongoing area of responsibility
     - 40_Resources/R1-R4 — if it's reference material
     - 50_Clippings/ — if it's external content
     - 60_Wiki/ — if it's an atomic concept definition
   - Specific subfolder within the destination
   - What metadata needs to be added

3. Present the routing plan:
   ```
   Inbox items: N
   Routing plan:
   - "科研日记.md" → 30_Areas/A1_科研学术/
   - "claude tricks.md" → 40_Resources/R1_专业知识/
   - "资产配置框架.md" → 30_Areas/A4_财务管理/
   ...
   ```

4. Ask user to confirm or adjust routing.

### Step 2: Process Each File

For each confirmed file:

1. Add/complete YAML frontmatter (title, created, modified, tags, description)
2. Use `obsidian search query="<key terms>"` to find related content → insert wikilinks
3. Add `## Related` section immediately after YAML frontmatter (before main content)
4. Move file to destination folder
5. Move any associated attach/ files

### Step 3: Report

List all moves made and new connections created.

## Routing Heuristics
- Research journals, experiment notes → A1_科研学术
- Career plans, job materials → A2_职业发展
- Self-improvement, habits → A3_个人成长
- Financial plans, budgets → A4_财务管理
- Technical references, tutorials → R1_专业知识
- Hobby notes → R2_兴趣爱好
- Book notes, summaries → R3_读书笔记
- Entertainment, lifestyle → R4_生活娱乐
- Web clippings, articles → 50_Clippings/<category>
- Standalone concepts → 60_Wiki/
- Active initiatives with goals → 20_Projects/

## Important Rules
- Ask user before moving any file — never auto-move without confirmation
- If a file is unclear, suggest 2-3 possible destinations and let user choose
- Preserve all original content
- Handle attach/ subdirectories if they exist alongside the note
