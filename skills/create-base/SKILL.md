---
name: create-base
description: Create Obsidian Bases database views with filters and formulas.
---

# Bases

Create .base database view files for the vault using Obsidian Bases format.

## Vault Path
~/Library/Mobile Documents/iCloud~md~obsidian/Documents/My_note

## Input
`/bases <preset>` or `/bases custom`

## Presets

### `/bases projects`
View: All projects by status
- Source: `20_Projects/`
- Columns: title, tags, completed, created, modified
- Filter: Show all
- Sort: completed ASC, modified DESC

### `/bases clippings`
View: Clippings by category
- Source: `50_Clippings/`
- Columns: title, tags, source, created
- Group by: folder (category)
- Sort: created DESC

### `/bases recent`
View: Recently modified notes
- Source: entire vault
- Columns: title, folder, tags, modified
- Sort: modified DESC
- Limit: 50

### `/bases stale`
View: Stale items needing attention
- Source: 20_Projects/, 00_Inbox/
- Columns: title, folder, modified, tags
- Filter: modified < 30 days ago
- Sort: modified ASC

### `/bases wiki`
View: Wiki index
- Source: `60_Wiki/`
- Columns: title, tags, aliases, created
- Sort: title ASC

### `/bases custom`
Interactive: ask user for source folder, columns, filters, sort order.

## Output

Create a `.base` file using Obsidian Bases JSON format. Use the obsidian:obsidian-bases skill for the exact file format specification.

Save to vault root or user-specified location.

## Important Rules
- Use the obsidian:obsidian-bases skill to understand the .base file format
- Ensure all referenced properties exist in file frontmatter
- Test that the .base file opens correctly in Obsidian
