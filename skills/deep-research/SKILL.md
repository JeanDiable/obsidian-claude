---
name: deep-research
description: Deep domain research — search vault + web, create comprehensive research notes with beginner-to-expert roadmap.
---

# Research

Systematic deep-dive into a topic. Combines vault knowledge with web research to produce a comprehensive research note with actionable learning roadmap.

## Vault Path
~/Library/Mobile Documents/iCloud~md~obsidian/Documents/My_note

## Input
User provides a topic: `/research <topic>`

## Workflow

### Phase 1: Plan (Present to User)

1. **Search existing vault knowledge**:
   ```bash
   obsidian search query="<topic terms>" limit=20
   ```

2. **Web research** using DuckDuckGo MCP:
   - Search for comprehensive overviews, tutorials, academic resources
   - Search for sub-topics and related domains
   - Search for practical guides and implementations

3. **Present research outline** to user:
   ```
   Topic: <topic>
   Existing vault knowledge: N files found
   Sub-topics identified:
   1. Sub-topic A
      - Detail a1
      - Detail a2
   2. Sub-topic B
   ...

   Proposed structure:
   - Main research note in: 30_Areas/A1_科研学术/ (or appropriate folder)
   - Wiki entries to create: [list]
   ```

4. Wait for user approval/adjustment.

### Phase 2: Execute

1. **Create main research note** at the approved location:
   ```markdown
   ---
   title: "<Topic> Research"
   created: YYYY-MM-DD
   modified: YYYY-MM-DD
   tags: [research, <domain-tags>]
   description: "Comprehensive research on <topic>"
   ---

   ## Related
   - [[ExistingVaultNote1]] — relationship
   - [[WikiEntry1]] — relationship

   # <Topic>

   ## Overview
   [High-level summary of the domain]

   ## Sub-topic 1: <Name>
   ### Background
   [Context and fundamentals]
   ### Key Concepts
   [Detailed explanation — reference [[WikiEntries]] where applicable]
   ### Detailed Outline
   [Leaf-level details — specific enough to act on]

   ## Sub-topic 2: <Name>
   [Same structure]

   ...

   ## Learning Roadmap
   ### Beginner (0-3 months)
   - [ ] Step 1: ...
   - [ ] Step 2: ...
   ### Intermediate (3-6 months)
   - [ ] Step 3: ...
   ### Advanced (6-12 months)
   - [ ] Step 4: ...

   ## Resources
   - [Resource 1](URL) — description
   - [Resource 2](URL) — description
   ```

2. **Create wiki entries** for key concepts discovered during research
3. **Update existing files** with backlinks to the new research note
4. **Link to vault knowledge** — connect findings to existing notes

### Phase 3: Report

Summarize what was created: main note, wiki entries, new connections.

## Important Rules
- Each sub-topic needs a detailed outline at the leaf level (specific enough to guide action)
- The roadmap must be actionable — concrete steps, not vague advice
- Always link to existing vault knowledge
- Create wiki entries for concepts that are reusable beyond this research
- Don't pad content — if a sub-topic is simple, keep it brief
- Use DuckDuckGo MCP for web research, cite sources with URLs
