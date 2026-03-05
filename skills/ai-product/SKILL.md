---
name: ai-product
description: Discover trending AI products from ProductHunt, HN, GitHub, Reddit. Based on OrbitOS /ai-products skill.
---

# AI Product Discovery

Find and curate trending AI product launches from multiple platforms.

## Vault Path
~/Library/Mobile Documents/iCloud~md~obsidian/Documents/My_note

## Data Sources

Use DuckDuckGo MCP to search each platform:

1. **Product Hunt**: Search "site:producthunt.com AI product launch today"
2. **Hacker News**: Search "site:news.ycombinator.com Show HN AI" + fetch `https://hn.algolia.com/api/v1/search?tags=show_hn&query=AI`
3. **GitHub**: Search "github trending AI" / "github.com/trending?language=python AI"
4. **Reddit**: Search "site:reddit.com r/artificial AI product new"

## Workflow

### Step 1: Fetch from Each Platform

For each source:
1. Search via DuckDuckGo MCP
2. Fetch top results using fetch_content
3. Extract: product name, URL, description, engagement metrics (upvotes, stars, comments)

### Step 2: Filter & Deduplicate

1. Filter for AI-related products (keywords: AI, ML, LLM, GPT, Claude, automation, agent, model, neural)
2. Merge duplicates appearing on multiple platforms (same product, different sources)
3. Rank by:
   - AI relevance
   - Engagement (normalized: PH upvotes/500, HN points/100, GitHub stars/1000)
   - Novelty
   - Practical utility

### Step 3: Generate Digest

Create `50_Clippings/AI_Products_YYYY-MM-DD.md`:

```markdown
---
title: "AI Product Launches YYYY-MM-DD"
created: YYYY-MM-DD
modified: YYYY-MM-DD
tags: [ai, products, digest]
description: "Trending AI products from PH, HN, GitHub, Reddit"
source: "Product Hunt, Hacker News, GitHub, Reddit"
---

# AI Product Launches — YYYY-MM-DD

## Top Picks
- **[Product Name]** ([PH](url) | [GH](url))
  Description. Why it matters. Engagement: X upvotes / Y stars.

## LLM & AI Models
- **Product** — Brief description. [Link](url)

## Developer Tools
- **Product** — Brief description. [Link](url)

## Productivity & Automation
- **Product** — Brief description. [Link](url)

## Open Source Highlights
- **Product** — Brief description. Stars: N. [GitHub](url)

## Other Notable
- **Product** — Brief description. [Link](url)

---
*Stats: X products found, Y duplicates merged, Z curated*
*Top source: [Platform] (N products)*
```

### Step 4: Save

Save to `50_Clippings/` root (uncategorized).

## Important Rules
- Do NOT categorize into subfolders — leave at root
- Include direct links to each product
- Note which platforms featured each product
- Focus on products that are NEW (launched in past 1-3 days)
- Keep descriptions actionable: what it does, why it matters
