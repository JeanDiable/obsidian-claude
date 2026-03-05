# Obsidian Claude Plugin — Vault Configuration

## Vault Location
```
~/Library/Mobile Documents/iCloud~md~obsidian/Documents/My_note
```

## Folder Structure

| Code | Folder | Purpose |
|------|--------|---------|
| 00 | 00_Inbox/ | Raw capture, unprocessed notes |
| 10 | 10_Diary/ | Daily notes (YYYY-MM-DD.md) |
| 20 | 20_Projects/ | Active projects with phases and goals |
| 30 | 30_Areas/ | Ongoing responsibilities (A1-A4 subfolders) |
| 40 | 40_Resources/ | Reference material (R1-R4 subfolders) |
| 50 | 50_Clippings/ | External content, categorized in subfolders |
| 60 | 60_Wiki/ | Atomic concept definitions, timeless and reusable |
| 70 | 70_Plans/ | Year/month/week planning documents |
| 80 | 80_Archive/ | Completed projects and archived content |
| 90 | 90_System/ | Templates, style guides, config |

### Area Subfolders (30_Areas/)
- A1_科研学术/ — Research & Academia
- A2_职业发展/ — Career Development
- A3_个人成长/ — Personal Growth
- A4_财务管理/ — Financial Management

### Resource Subfolders (40_Resources/)
- R1_专业知识/ — Professional Knowledge
- R2_兴趣爱好/ — Hobbies & Interests
- R3_读书笔记/ — Book Notes
- R4_生活娱乐/ — Life & Entertainment

### Clippings Subfolders (50_Clippings/)
- AI/
- 金融/ (Finance)
- 科技/ (Technology)
- 自我成长/ (Self-growth)
- 社会学/ (Sociology)
- 心理学/ (Psychology)
- 投资/ (Investment)

## Metadata Schema

### Standard (all files)
```yaml
---
title: "Human-readable title"
created: YYYY-MM-DD
modified: YYYY-MM-DD
tags: [tag1, tag2, tag3]  # 1-5 tags
description: "Brief content summary in 1-2 sentences"
---
```

### Projects (additional)
```yaml
completed: false  # or true
```

### Clippings (additional)
```yaml
source: "URL or source name"
```

### Wiki entries (additional)
```yaml
aliases: [alternate name1, alternate name2]
```

## Naming Conventions
- Daily notes: `YYYY-MM-DD.md`
- Wiki entries: `ConceptName.md` (PascalCase or natural Chinese name)
- Projects: Natural name, no special prefix
- Plans: `YYYY年M月计划.md`, `YYYYMM第N周.md`

## Wikilink Conventions
- Use `[[FileName]]` without path (Obsidian resolves automatically)
- For disambiguation: `[[folder/FileName]]`
- Display text: `[[FileName|Display Text]]`

## Backlinking Protocol
When creating or modifying any file:
1. Search PARA (20-40) + 50_Clippings + 60_Wiki for related content
2. Insert `[[wikilinks]]` inline where concepts naturally appear in text
3. Add/update `## Related` section immediately after YAML frontmatter (before main content) with categorized links
4. When linking to a wiki entry, also update the wiki entry's `## Related` to point back
5. If a significant concept has no wiki entry, consider creating one in 60_Wiki/

## Content Language
- Vault content is ~90% Chinese, ~10% English
- Metadata fields (title, description, tags) should match the content language
- Tags can be bilingual
- Skill instructions are in English, but output follows content language

## Obsidian CLI (MANDATORY)

**ALWAYS use Obsidian CLI and obsidian-skills for ALL vault operations.** Do NOT fall back to standard file reading (Read tool), grep, shell commands, or direct file system access for vault files. The Obsidian CLI is the single source of truth.

Required commands:
- `obsidian search query="term"` — full-text search (NOT grep/rg)
- `obsidian read path="folder/file.md"` — read file content (NOT Read tool/cat)
- `obsidian create name="Name" content="..." silent` — create files (NOT Write tool)
- `obsidian append file="Name" content="..."` — append to files (NOT Edit tool)
- `obsidian property:set name="key" value="val" type=text path="folder/file.md"` — set frontmatter
- `obsidian property:read name="key" path="folder/file.md"` — read frontmatter
- `obsidian backlinks file="Name"` — find backlinks
- `obsidian tags` — list all tags

For file listing within vault folders, use `ls` via Bash since Obsidian CLI doesn't have a list command.

**No exceptions.** If Obsidian is not running, prompt the user to open it rather than falling back to direct file access.
