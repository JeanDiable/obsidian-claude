---
name: organize-clippings
description: Consolidate and categorize clippings — add metadata, summarize, create wikilinks, move to subfolders.
---

# Organize Clippings

Process uncategorized clippings in 50_Clippings/ root — add metadata, consolidate content, create connections, and move to category subfolders.

## Vault Path
~/Library/Mobile Documents/iCloud~md~obsidian/Documents/My_note

## Workflow

### Phase 1: Scan

1. List all .md files directly in `50_Clippings/` root (not in subfolders — those are already categorized)
2. For each file, read content and determine:
   - Current metadata state (missing/partial/complete)
   - Best category: AI, 金融, 科技, 自我成长, 社会学, 心理学, 投资, or suggest a new category
   - Content quality: needs condensing? already concise?

3. Present plan to user:
   ```
   Uncategorized clippings: N
   Category breakdown:
   - AI: X files
   - 金融: Y files
   - 科技: Z files
   ...
   Files needing condensing: M
   ```

4. Ask user to confirm categorization and any new subfolder creation.

### Phase 2: Process Each File

For each clipping:

1. **Fix metadata**: Add/complete YAML frontmatter:
   ```yaml
   ---
   title: "..."
   created: YYYY-MM-DD
   modified: YYYY-MM-DD (today)
   tags: [tag1, tag2, tag3]
   description: "1-2 sentence summary"
   source: "URL or source name"
   ---
   ```

2. **Condense if needed**: If the content is excessively verbose (web scraping artifacts, excessive formatting):
   - Preserve all substantive information
   - Remove navigation elements, ads, redundant text
   - Restructure into clean markdown with headers
   - Keep the original meaning and key details

3. **Build connections**:
   - Use `obsidian search query="<key terms>"` to find related content in vault
   - Insert `[[wikilinks]]` inline where concepts match existing files
   - Add/update `## Related` section immediately after YAML frontmatter (before main content)
   - Update any linked Wiki entries to point back

### Phase 3: Move to Subfolders

For each processed file:

1. Move to the determined category subfolder: `50_Clippings/<Category>/`
2. If file has an attach/ subfolder with images, move those too
3. Verify the file is accessible after move

### Phase 4: Report

Generate summary saved to 90_System/Reports/organize-clippings-YYYY-MM-DD.md

## Category Determination Rules
- AI: Machine learning, LLMs, neural networks, AI tools, AI research
- 金融: Markets, stocks, crypto, trading, financial instruments
- 科技: Software, hardware, tech products, programming
- 自我成长: Psychology, habits, productivity, personal development
- 社会学: Social dynamics, culture, society, relationships
- 心理学: Cognitive science, mental health, behavioral science
- 投资: Investment strategies, portfolio management, asset allocation
- If a clipping spans multiple categories, choose the primary one and add secondary categories as tags

## Important Rules
- NEVER delete original content — condense means restructure, not remove information
- Preserve source attribution
- Process in batches of 5-10 files
- Match language of metadata to content language
