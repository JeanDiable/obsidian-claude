---
name: ai-newsletter
description: Curate and summarize AI newsletter content. Based on OrbitOS /ai-newsletters skill.
---

# AI Newsletter

Fetch, deduplicate, rank, and summarize AI newsletter content into a digest clipping.

## Vault Path
~/Library/Mobile Documents/iCloud~md~obsidian/Documents/My_note

## Data Sources

Use DuckDuckGo MCP to search for recent AI news and newsletters:
- Search: "AI newsletter this week" / "AI news digest today"
- Search: "LLM updates this week" / "machine learning news"
- Search: "AI tools new releases" / "AI research breakthroughs"
- Target sources: TLDR AI, The Rundown AI, Ben's Bites, Import AI, The Batch

Also try fetching RSS feeds directly if available:
- TLDR AI: `https://bullrich.dev/tldr-rss/ai.rss`
- The Rundown AI: `https://rss.beehiiv.com/feeds/2R3C6Bt5wj.xml`

## Workflow

### Step 1: Fetch Content

1. Search for AI newsletters and digests from the past 1-3 days
2. Fetch content from found URLs using DuckDuckGo MCP's fetch_content
3. Try RSS feeds as backup sources

### Step 2: Process & Deduplicate

1. Extract individual news items from fetched content
2. Deduplicate: merge items with 80%+ word overlap in titles/descriptions
3. Rank by:
   - AI relevance (keywords: LLM, GPT, Claude, agents, ML, transformer, diffusion)
   - Recency (prefer today > yesterday > older)
   - Novelty (not covered in previous digests)
   - Practical impact (tools, products > pure research > opinion)

### Step 3: Generate Digest

Create `50_Clippings/AI_Newsletter_YYYY-MM-DD.md`:

```markdown
---
title: "AI Newsletter Digest YYYY-MM-DD"
created: YYYY-MM-DD
modified: YYYY-MM-DD
tags: [ai, newsletter, digest]
description: "Curated AI news digest"
source: "Multiple AI newsletters"
---

# AI Newsletter Digest — YYYY-MM-DD

## Top Picks
- **[Title]** ([Source](URL))
  Summary. Why it matters.

## AI Models & Research
- **Item** — Brief summary

## Tools & Products
- **Item** — Brief summary

## Industry & Business
- **Item** — Brief summary

## Other Notable
- **Item** — Brief summary

---
*Stats: X items fetched, Y duplicates merged, Z curated items*
*Sources: [list of sources]*
```

### Step 4: Save

Save to `50_Clippings/` root (uncategorized — user sorts manually later).

## Error Handling
- If DuckDuckGo search fails: note the error, try alternative search terms
- If no fresh content found: note "No new content found for today"
- If RSS feeds fail: rely on web search only

## Important Rules
- Do NOT categorize the output into Clippings subfolders — leave at root for manual sorting
- Keep summaries concise — 1-2 sentences per item
- Always include source URLs
- Deduplicate aggressively — same story from different sources should merge
