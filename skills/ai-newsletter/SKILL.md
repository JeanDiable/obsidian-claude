---
name: ai-newsletter
description: Curate AI newsletter content with smart deduplication and ranking. Use when user invokes /ai-newsletter.
---

# AI Newsletter Curation

Fetch, deduplicate, and rank AI newsletter content into a daily digest.

## Vault Path
~/Library/Mobile Documents/iCloud~md~obsidian/Documents/My_note

## RSS Sources

- **TLDR AI**: `https://bullrich.dev/tldr-rss/ai.rss`
- **The Rundown AI**: `https://rss.beehiiv.com/feeds/2R3C6Bt5wj.xml`

## Workflow

1. **Check cache**: Look for `50_Clippings/AI/Newsletters/YYYY-MM/YYYY-MM-DD-摘要.md`. If exists with today's date, return cached content.

2. **Fetch feeds**: Use WebFetch on both RSS URLs. Extract title, link, pubDate, description for each item. If RSS fails, fall back to DuckDuckGo MCP search for "AI newsletter this week", "LLM updates this week", targeting TLDR AI, The Rundown AI, Ben's Bites, Import AI, The Batch.

3. **Deduplicate**: Merge items with similar titles (80%+ word overlap). Keep longer description, track both sources.

4. **Rank items** by:
   - AI relevance (LLM, GPT, Claude, agents, ML, transformer, diffusion keywords)
   - Productivity relevance (workflow, automation, tools, PKM)
   - Recency (newer = higher)
   - Novelty (check recent archives, penalize repeats)
   - Practical impact (tools, products > pure research > opinion)

5. **Generate digest**: Use the template below. Include:
   - 精选推荐 (3-5 highest scoring) with content creation angles
   - AI动态 section
   - 生产力工具 section
   - Stats footer

6. **Save files** using Obsidian CLI (`obsidian create`):
   - `50_Clippings/AI/Newsletters/YYYY-MM/YYYY-MM-DD-摘要.md` (curated digest)
   - `50_Clippings/AI/Newsletters/YYYY-MM/原始数据/YYYY-MM-DD_TLDR-AI-Raw.md` (raw feed)
   - `50_Clippings/AI/Newsletters/YYYY-MM/原始数据/YYYY-MM-DD_Rundown-AI-Raw.md` (raw feed)

## Digest Template

```markdown
---
title: "AI Newsletter 摘要 YYYY-MM-DD"
created: YYYY-MM-DD
modified: YYYY-MM-DD
tags: [ai, newsletter, digest]
description: "AI newsletter curated digest"
source: "TLDR AI, The Rundown AI"
type: newsletter-digest
item_count: N
duplicates_merged: N
top_topics: [话题1, 话题2, 话题3]
raw_sources:
  - "[[YYYY-MM-DD_TLDR-AI-Raw]]"
  - "[[YYYY-MM-DD_Rundown-AI-Raw]]"
---

## Related
- [[AI]]

# AI Newsletter 摘要: YYYY-MM-DD

> 来源: TLDR AI, The Rundown AI
> 原始 Newsletter: [[YYYY-MM-DD_TLDR-AI-Raw]] | [[YYYY-MM-DD_Rundown-AI-Raw]]

## 精选推荐 (内容创作机会)

- [ ] **[标题]** ([来源])
  链接: [URL]
  亮点: [相关性说明]
  角度: [教程 | 工具评测 | 趋势分析 | 对比]

## AI动态

- **[标题]** ([来源])
  [简要总结]
  链接: [URL]

## 生产力工具

- **[标题]** ([来源])
  [简要总结]
  链接: [URL]

## 其他值得关注

- **[标题]** ([来源]) - 链接: [URL]

---

**统计:**
- 获取条目总数: X
- 合并重复数: Y
- 最终精选数: Z
```

## Content Angle Suggestions

When adding angles to 精选推荐:
- High AI relevance → "教程机会", "趋势分析"
- High productivity relevance → "工作流展示", "工具评测"
- Novel + trending → "抢先报道", "思想引领"

## Error Handling

- One feed down: Continue with other, note in digest
- Both RSS down: Fall back to DuckDuckGo MCP search
- All sources fail: Create minimal digest noting "今日无新内容"
- Empty feeds: Create minimal digest with explanation

## Important Rules

- Keep summaries concise — 1-2 sentences per item
- Always include source URLs
- Deduplicate aggressively — same story from different sources should merge
- Save raw data alongside curated digest for reference
- Use Obsidian CLI for all file operations
