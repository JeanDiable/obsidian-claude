---
name: wiki-builder
description: Extract atomic concepts from vault files and build interconnected wiki entries in 60_Wiki/. One-time but re-runnable incrementally.
---

# Build Wiki

Scan PARA + Clippings to extract recurring concepts, create atomic wiki entries, and establish bidirectional links.

## Vault Path
~/Library/Mobile Documents/iCloud~md~obsidian/Documents/My_note

## Workflow

### Phase 1: Concept Extraction

1. Scan all .md files in:
   - 20_Projects/
   - 30_Areas/ (recursively)
   - 40_Resources/ (recursively)
   - 50_Clippings/ (recursively)

2. For each file, extract:
   - Key concepts, technical terms, and domain-specific vocabulary
   - Recurring themes across multiple files
   - Named entities (frameworks, methodologies, theories, tools)
   - Concepts that are referenced but not well-defined in any single file

3. Build a concept list with:
   - Concept name
   - Frequency (how many files mention it)
   - Source files
   - Brief definition extracted from context

### Phase 2: Deduplicate & Cluster

1. Merge synonyms and near-duplicates (e.g., "联邦学习" and "Federated Learning")
2. Cluster related concepts into domains
3. Filter: only create wiki entries for concepts appearing in 2+ files OR that represent a fundamental domain concept
4. Check existing 60_Wiki/ entries to avoid duplicates

5. Present the plan:
   ```
   Concepts extracted: N
   After dedup: M
   New wiki entries to create: K
   Existing wiki entries to update: J

   Top concepts:
   - Federated Learning (8 mentions)
   - Transformer (6 mentions)
   - ...
   ```

6. Ask user to confirm.

### Phase 3: Create Wiki Entries

For each approved concept:

1. Create `60_Wiki/<ConceptName>.md`:
   ```markdown
   ---
   title: "Concept Name"
   created: YYYY-MM-DD
   modified: YYYY-MM-DD
   tags: [domain-tag]
   description: "One-line definition"
   aliases: [alternate names, 中文别名]
   ---

   ## Related
   - [[SourceFile1]] — context of how this concept appears
   - [[SourceFile2]] — context of how this concept appears
   - [[OtherWikiEntry]] — relationship to another concept

   # Concept Name

   [Atomic definition: 2-5 sentences explaining the concept clearly and concisely.
   Should be understandable without reading the source files.]

   ## Key Points
   - Point 1
   - Point 2
   - Point 3
   ```

2. Keep definitions atomic — one concept per file, timeless, reusable

### Phase 4: Backlink Source Files

For each source file that mentions a wiki concept:

1. Add `[[WikiConceptName]]` wikilinks inline in the text where the concept naturally appears
   - Don't over-link: link the first meaningful occurrence, not every mention
2. Add the wiki entry to the file's `## Related` section (located after frontmatter) if not already there

### Phase 5: Build Wiki Index

Create `60_Wiki/_Index.md`:
```markdown
---
title: "Wiki Index"
created: YYYY-MM-DD
modified: YYYY-MM-DD
tags: [wiki, index]
description: "Master index of all atomic concept entries"
---

# Wiki Index

## By Domain

### AI & Machine Learning
- [[Transformer]]
- [[FederatedLearning]]
...

### Finance & Investment
- [[AssetAllocation]]
...

[etc. for each domain]

## Recently Added
- [[Concept1]] (YYYY-MM-DD)
- [[Concept2]] (YYYY-MM-DD)
```

### Phase 6: Report

Save to 90_System/Reports/build-wiki-YYYY-MM-DD.md

## Important Rules
- Wiki entries should be ATOMIC: one concept, one file
- Definitions should be TIMELESS: not tied to a specific project or date
- Definitions should be SELF-CONTAINED: understandable without context
- Don't create wiki entries for overly specific or one-time concepts
- Bilingual aliases are important (e.g., aliases: [Federated Learning, 联邦学习])
- Process in batches to manage context window
- When re-running, only process new or modified files since last run
