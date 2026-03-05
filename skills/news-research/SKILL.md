---
name: news-research
description: Multi-domain news research with cross-domain analysis and investment impact assessment.
---

# News Research

Given news headlines, research across domains (politics, economics, finance, tech, AI), find connections, and analyze investment implications.

## Vault Path
~/Library/Mobile Documents/iCloud~md~obsidian/Documents/My_note

## Input
User provides one or more headlines: `/news-research <headline1>; <headline2>`

## Workflow

### Step 1: Research Each Headline

For each provided headline:

1. **Search for related coverage** using DuckDuckGo MCP:
   - Direct search for the headline
   - Search in specific domains: politics, economics, finance, tech, AI
   - Search for analysis and opinion pieces

2. **Summarize findings** per domain:
   - Core facts and developments
   - Key highlights and non-obvious insights
   - Expert opinions if found

### Step 2: Cross-Domain Analysis

1. Identify connections between the news items across domains:
   - How does a political event affect technology?
   - How does a tech development affect financial markets?
   - What second-order effects exist?

2. Search for related developments in adjacent domains that the user may not have considered

### Step 3: Investment Analysis

Analyze implications for:

1. **Short-term (1-7 days)**:
   - Immediate market reactions
   - Sentiment shifts
   - Specific sectors/stocks affected

2. **Mid-term (1-3 months)**:
   - Industry trend implications
   - Regulatory impacts
   - Supply chain effects

3. **Long-term (6-12+ months)**:
   - Structural changes
   - New market opportunities
   - Technology adoption curves

Focus on: US stock market (specific sectors/companies) and Bitcoin/crypto.

### Step 4: Generate Report

Create `50_Clippings/News_Research_YYYY-MM-DD_<brief-topic>.md`:

```markdown
---
title: "News Research: <Brief Topic>"
created: YYYY-MM-DD
modified: YYYY-MM-DD
tags: [news, research, investment]
description: "Multi-domain analysis of <headlines>"
---

# News Research — YYYY-MM-DD

## Headlines Analyzed
1. Headline 1
2. Headline 2

## Domain Analysis

### 政治 (Politics)
[Relevant findings, impacts]

### 经济 (Economics)
[Relevant findings, impacts]

### 金融 (Finance)
[Relevant findings, impacts]

### 科技 (Technology)
[Relevant findings, impacts]

### AI
[Relevant findings, impacts]

## Cross-Domain Connections
- Connection 1: How X in domain A affects Y in domain B
- Connection 2: ...

## Investment Implications

### 短期影响 (Short-term: 1-7 days)
**US Stocks**: [Analysis with specific sectors/companies]
**BTC/Crypto**: [Analysis]

### 中期影响 (Mid-term: 1-3 months)
**US Stocks**: [Analysis]
**BTC/Crypto**: [Analysis]

### 长期影响 (Long-term: 6-12+ months)
**US Stocks**: [Analysis]
**BTC/Crypto**: [Analysis]

## Sources
- [Source 1](URL)
- [Source 2](URL)
```

### Step 5: Save

Save to `50_Clippings/` (at root, uncategorized).

## Important Rules
- Be balanced — present multiple viewpoints, not just bullish/bearish
- Cite specific sources for factual claims
- Investment analysis should be analytical, not predictive — discuss possibilities, not certainties
- Cross-domain analysis is the most valuable part — dig for non-obvious connections
- Keep summaries concise but insightful
- Include specific company/sector names when discussing stock implications
