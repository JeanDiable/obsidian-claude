---
name: organize-para
description: Batch organize PARA files — add metadata, create wikilinks, build connections. One-time but re-runnable.
---

# Organize PARA

Batch process all files in PARA folders (20_Projects, 30_Areas, 40_Resources, 80_Archive) to add metadata and create interconnections.

## Vault Path
~/Library/Mobile Documents/iCloud~md~obsidian/Documents/My_note

## Workflow

### Phase 1: Scan & Plan

1. List all .md files in these vault folders:
   - 20_Projects/
   - 30_Areas/ (recursively including A1-A4 subfolders)
   - 40_Resources/ (recursively including R1-R4 subfolders)
   - 80_Archive/

2. For each file, check if it has YAML frontmatter. Categorize into:
   - **Missing metadata**: No frontmatter at all
   - **Partial metadata**: Has frontmatter but missing required fields (title, created, modified, tags, description)
   - **Complete**: Has all required fields

3. Present a summary to the user:
   ```
   Files scanned: N
   Missing metadata: X
   Partial metadata: Y
   Complete: Z
   ```

4. Ask user to confirm before proceeding.

### Phase 2: Add/Fix Metadata

For each file needing metadata work:

1. Read the full file content
2. Generate appropriate metadata:
   - **title**: Extract from first heading or filename
   - **created**: Use file creation date if available, or today's date
   - **modified**: Today's date
   - **tags**: Generate 1-5 relevant tags from content analysis
   - **description**: Write a 1-2 sentence summary of the content
   - **completed**: (Projects only) Determine from content if project is done
3. Add or update YAML frontmatter at the top of the file
4. If file already has partial frontmatter, preserve existing correct values and fill in missing ones

### Phase 3: Build Connections

For each processed file:

1. Extract key concepts, terms, and topics from the content
2. Search the entire vault (PARA + 50_Clippings + 60_Wiki) for files with related content:
   - Use `obsidian search query="<term>"` to find matching files
   - Check existing tags for overlap
   - Look for files in the same domain/area
3. Insert `[[wikilinks]]` inline in the text where concepts naturally appear and a matching file exists
4. Add or update a `## Related` section immediately after the YAML frontmatter (before the main content):
   ```markdown
   ---
   (frontmatter)
   ---

   ## Related
   - [[RelatedFile1]] — brief description of relationship
   - [[RelatedFile2]] — brief description of relationship

   (rest of file content)
   ```
5. If linking to a 60_Wiki entry, also update that wiki entry's Related section to point back

### Phase 4: Report

Generate a summary report showing:
- Total files processed
- Metadata added/updated count
- New wikilinks created count
- List of all new connections made

Save report to 90_System/Reports/organize-para-YYYY-MM-DD.md

## Important Rules
- NEVER delete or overwrite existing content — only add metadata and links
- Preserve the original file structure and content
- If unsure about a tag or connection, err on the side of not adding it
- Process files in batches of 10-15 to stay within context limits
- Use subagents for parallel processing when possible
- All metadata and descriptions should match the content language (Chinese or English)
