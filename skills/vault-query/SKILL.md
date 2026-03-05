---
name: vault-query
description: Query the vault (PARA + Clippings + Wiki) and get answers with source citations.
---

# Ask

Quick vault Q&A — search, synthesize, cite.

## Vault Path
~/Library/Mobile Documents/iCloud~md~obsidian/Documents/My_note

## Input
User provides a question as the skill argument: `/ask <question>`

## Workflow

1. **Parse the question** — identify key search terms and concepts

2. **Search the vault** — search across all content folders:
   ```bash
   obsidian search query="<search terms>" limit=20
   ```

3. **Read relevant files** — read the top 5-10 most relevant matches

4. **Synthesize answer** — combine information from multiple sources into a clear, concise answer:
   - Cite sources using `[[wikilinks]]`
   - If sources disagree, note the disagreement
   - If information is insufficient, say so

5. **Output** — respond directly to the user with the answer. No file creation unless the answer reveals a substantial insight worth saving (in which case, offer to save it).

## Important Rules
- Be fast and lightweight — this is not a research skill
- Always cite sources with [[wikilinks]]
- If the vault doesn't contain relevant info, say so honestly rather than hallucinating
- Search scope: 20_Projects, 30_Areas, 40_Resources, 50_Clippings, 60_Wiki (not Inbox, Archive, or System)
