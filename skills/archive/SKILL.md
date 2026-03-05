---
name: archive
description: Review all projects, identify completed/stale ones, generate wrap-ups, and move to archive.
---

# Archive

Check all projects for completion status, generate wrap-up summaries, and archive finished work.

## Vault Path
~/Library/Mobile Documents/iCloud~md~obsidian/Documents/My_note

## Workflow

### Step 1: Scan Projects

1. List all items in `20_Projects/`
2. For each project, read frontmatter and content to determine:
   - **Completed**: `completed: true` in frontmatter, or content clearly indicates finished
   - **Active**: Clear ongoing work, recent modifications
   - **Stale**: No updates in 30+ days, unclear status
   - **Unknown**: No metadata, can't determine

3. Present status report:
   ```
   Projects: N total
   Completed (ready to archive): X
   Active: Y
   Stale (needs review): Z
   Unknown status: W
   ```

### Step 2: User Review

For stale and unknown projects, ask user one by one:
- Archive it? (completed)
- Keep active? (still working on it)
- Delete? (abandoned, not worth keeping)

### Step 3: Archive Completed Projects

For each project to archive:

1. Set `completed: true` and update `modified` date in frontmatter
2. Generate a wrap-up section at the end of the file:
   ```markdown
   ## Archive Summary
   - **Archived**: YYYY-MM-DD
   - **Duration**: Started YYYY-MM-DD, ended YYYY-MM-DD
   - **Outcome**: [Brief summary of what was accomplished]
   - **Key learnings**: [What was learned]
   ```
3. Move to `80_Archive/`
4. Update any files that link to this project (wikilinks still work regardless of folder)

### Step 4: Report

List all actions taken.

## Important Rules
- Never auto-archive without user confirmation
- Preserve all content — archiving is moving, not deleting
- Update the modified date when archiving
- Wikilinks to archived projects still work (Obsidian resolves by filename)
