---
name: ai-product
description: Curate AI product launches from Product Hunt, Hacker News, GitHub, and Techmeme. Use when user invokes /ai-product.
---

# AI Product Discovery

Fetch, deduplicate, and rank AI product launches from multiple sources.

## Vault Path
~/Library/Mobile Documents/iCloud~md~obsidian/Documents/My_note

## Sources

| Source | Method | Notes |
|--------|--------|-------|
| Product Hunt | WebFetch `https://www.producthunt.com/feed` + DuckDuckGo search "site:producthunt.com AI product launch today" | Filter for AI-related |
| Hacker News | WebFetch `https://hn.algolia.com/api/v1/search?tags=show_hn&numericFilters=created_at_i>TIMESTAMP` | Show HN posts, 24h window. Replace TIMESTAMP with Unix timestamp for 24h ago |
| GitHub Trending | WebFetch `https://mshibanami.github.io/GitHubTrendingRSS/daily/python.xml` + DuckDuckGo search "github trending AI" | Python repos |
| Techmeme | WebFetch `https://techmeme.com/river` | Product announcements |
| Reddit | DuckDuckGo search "site:reddit.com r/artificial AI product new" | Supplementary |

## Workflow

1. **Check cache**: Look for `50_Clippings/AI/产品发布/YYYY-MM/YYYY-MM-DD-摘要.md`. If exists with today's date, return cached.

2. **Fetch sources**: Use WebFetch on each URL. For sources without RSS, use DuckDuckGo MCP search + fetch_content. Extract product name, URL, description, and engagement metrics (votes/points/stars).

3. **Filter**: Keep only AI-related products (keywords: AI, ML, LLM, GPT, Claude, automation, agent, model, neural, transformer, diffusion).

4. **Deduplicate**: Same product across sources = merge. Keep best description, combine metrics, track all sources.

5. **Rank by**:
   - AI relevance
   - Engagement (normalize: PH votes/500, HN points/100, GH stars/1000)
   - Content potential (tutorial-friendly, review-worthy, open source bonus)
   - Recency and novelty

6. **Generate digest**: Use the template below. Sections:
   - 精选推荐 (3-5) with content angles
   - LLM与AI模型
   - 开发者工具
   - 生产力与自动化
   - 开源亮点

7. **Save files** using Obsidian CLI (`obsidian create`):
   - `50_Clippings/AI/产品发布/YYYY-MM/YYYY-MM-DD-摘要.md`
   - `50_Clippings/AI/产品发布/YYYY-MM/原始数据/YYYY-MM-DD_ProductHunt-Raw.md`
   - `50_Clippings/AI/产品发布/YYYY-MM/原始数据/YYYY-MM-DD_HackerNews-Raw.md`
   - `50_Clippings/AI/产品发布/YYYY-MM/原始数据/YYYY-MM-DD_GitHub-Raw.md`

## Digest Template

```markdown
---
title: "AI产品发布 YYYY-MM-DD"
created: YYYY-MM-DD
modified: YYYY-MM-DD
tags: [ai, products, digest]
description: "Trending AI products from PH, HN, GitHub, Techmeme"
source: "Product Hunt, Hacker News, GitHub, Techmeme"
type: product-digest
product_count: N
duplicates_merged: N
top_categories: [LLM工具, 开发者工具, 自动化]
raw_sources:
  - "[[YYYY-MM-DD_ProductHunt-Raw]]"
  - "[[YYYY-MM-DD_HackerNews-Raw]]"
  - "[[YYYY-MM-DD_GitHub-Raw]]"
---

## Related
- [[AI]]

# AI产品发布: YYYY-MM-DD

> 来源: Product Hunt, Hacker News, GitHub, Techmeme
> 原始数据: [[YYYY-MM-DD_ProductHunt-Raw]] | [[YYYY-MM-DD_HackerNews-Raw]] | [[YYYY-MM-DD_GitHub-Raw]]

## 精选推荐 (内容创作机会)

- [ ] **[产品名称]** ([来源])
  链接: [URL]
  描述: [功能说明]
  数据: [PH: X票 | HN: Y分 | GH: Z星]
  亮点: [独特性/相关性]
  角度: [教程 | 评测 | 对比 | 深度分析]

## LLM与AI模型

- **[产品]** ([来源])
  [简要描述]
  数据: [关键指标]
  链接: [URL]

## 开发者工具

- **[产品]** ([来源])
  [简要描述]
  数据: [关键指标]
  链接: [URL]

## 生产力与自动化

- **[产品]** ([来源])
  [简要描述]
  链接: [URL]

## 开源亮点

- **[仓库名]** (GitHub)
  [描述]
  数据: [星标数]
  链接: [URL]

## 其他值得关注

- **[产品]** ([来源]) - [URL]

---

**统计:**
- 总发现产品数: X
- 合并重复数: Y
- 最终精选数: Z
- 主要来源: [来源] (N个产品)
```

## Content Angle Logic

When adding angles to 精选推荐:
- High engagement + tutorial-friendly → "教程机会"
- Novel + early stage → "抢先报道优势"
- Open source + complex → "深度分析"
- SaaS + practical → "工具评测"
- Similar to existing tool → "对比 vs [竞品]"

## Error Handling

- Source down: Continue with others, note in digest
- <2 sources available: Fall back to DuckDuckGo search only
- Empty results: Create minimal digest noting "今日无新AI产品"

## Important Rules

- Focus on products launched in the past 1-3 days
- Include direct links to each product
- Note which platforms featured each product
- Keep descriptions actionable: what it does, why it matters
- Save raw data alongside curated digest
- Use Obsidian CLI for all file operations
