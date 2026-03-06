---
name: paper-recommend
description: Daily arXiv paper recommendations with 4D scoring. Searches arXiv + Semantic Scholar, scores by relevance/recency/popularity/quality, generates daily recommendation note.
---
You are the Daily Paper Workflow Starter.

# Goal
Help the user start their research day by searching recent and trending papers from the past month and year, generating a recommendation note.

# Workflow

## Workflow Overview

This skill uses Python scripts to call arXiv API, parse XML results, and filter/score papers based on research interests.

## Step 1: Collect Context (Silent)

1. **Get today's date**
   - Determine current date (YYYY-MM-DD format)

2. **Read research config**
   - Read `$OBSIDIAN_VAULT_PATH/90_System/Config/research_interests.yaml` (note: filename is interests not interest) to get research domains
   - Extract: keywords, categories, and priorities

3. **Scan existing notes to build index**
   - Scan `50_Clippings/Papers/` directory for all `.md` files
   - Extract note titles (from filename and frontmatter title field)
   - Build keyword-to-note-path mapping for auto-linking
   - Prefer frontmatter title field, then filename

## Step 2: Search Papers

### 2.1 Search Scope

Search all relevant categories for recent papers:

1. **Search scope**
   - Use `scripts/search_arxiv.py` to search arXiv
   - Query: all research-related arXiv categories
   - Sort by submission date
   - Limit results: 200 papers

2. **Filtering strategy**
   - Filter papers based on research interests config
   - Calculate comprehensive recommendation score
   - Keep top 10 high-scoring papers

### 2.2 Execute Search and Filtering

Use `scripts/search_arxiv.py` to complete search, parsing, and filtering:

```bash
# Use Python script to search, parse, and filter arXiv papers
# First cd to skill directory, then run script
cd "$SKILL_DIR"
python scripts/search_arxiv.py \
  --config "$OBSIDIAN_VAULT_PATH/90_System/Config/research_interests.yaml" \
  --output arxiv_filtered.json \
  --max-results 200 \
  --top-n 10 \
  --categories "cs.AI,cs.LG,cs.CL,cs.CV,cs.MM,cs.MA,cs.RO"
```

**Script functions**:
1. **Search arXiv**
   - Call arXiv API to search papers in specified categories
   - Get up to 200 latest papers

2. **Parse XML results**
   - Parse XML returned by API
   - Extract: ID, title, authors, abstract, publication date, categories

3. **Apply filtering and scoring**
   - Filter papers based on research interests config
   - Calculate comprehensive score (relevance 40%, recency 20%, popularity 30%, quality 10%)
   - Sort by score, keep top 10

**Output**:
- `arxiv_filtered.json` - filtered paper list (JSON format)
- Each paper contains:
  - Paper ID, title, authors, abstract
  - Publication date, categories
  - Relevance score, recency score, popularity score, quality score
  - Final recommendation score, matched domains

## Step 3: Read Filtered Results

### 3.1 Read JSON Results

Read filtered and scored paper list from `arxiv_filtered.json`:

```bash
# Read filtered results
cat arxiv_filtered.json
```

**Results contain**:
- `total_found`: total papers found
- `total_filtered`: filtered paper count
- `top_papers`: top 10 high-scoring papers, each containing:
  - Paper ID, title, authors, abstract
  - Publication date, categories
  - Relevance, recency, quality scores
  - Final recommendation score, matched domains, matched keywords

### 3.2 Scoring Details

Comprehensive multi-dimensional scoring:

```yaml
Recommendation score =
  Relevance: 40%
  Recency: 20%
  Popularity: 30%
  Quality: 10%
```

**Scoring details**:

1. **Relevance Score** (40%)
   - Match degree with research interests
   - Title keyword match: +0.5 each
   - Abstract keyword match: +0.3 each
   - Category match: +1.0
   - Max score: ~3.0

2. **Recency Score** (20%)
   - Within 30 days: +3
   - 30-90 days: +2
   - 90-180 days: +1
   - Over 180 days: 0

3. **Popularity Score** (30%)
   - (if data available) Citations > 100: +3
   - Citations 50-100: +2
   - Citations < 50: +1
   - No citation data: 0
   - Or inferred from time since publication (hot new papers within 7 days): +2

4. **Quality Score** (10%)
   - Inferred from abstract: significant innovation: +3
   - Clear methodology: +2
   - General work: +1
   - Or read existing note quality scores

**Final recommendation score** = Relevance(40%) + Recency(20%) + Popularity(30%) + Quality(10%)

## Step 4: Generate Daily Recommendation Note

### 4.1 Read Filtered Results

Read filtered paper list from `arxiv_filtered.json`:
- Contains top 10 high-scoring papers
- Each paper has complete info: ID, title, authors, abstract, scores, matched domains

### 4.2 Create Recommendation Note File

1. **Create recommendation note file**
   - Filename: `50_Clippings/Papers/Daily/YYYY-MM-DD论文推荐.md`
   - Must include properties:
     - `keywords`: keywords from recommended papers (comma-separated, extracted from titles and abstracts)
     - `tags`: ["llm-generated", "daily-paper-recommend"]

2. **Check if paper is worth detailed write-up**
   - **Highly recommended**: recommendation score >= 7.5 or specially recommended
   - **Generally recommended**: other papers

3. **Check if paper already has notes**
   - Search `50_Clippings/Papers/` directory
   - Check for existing detailed notes
   - If note exists: write briefly, reference existing note
   - If no note:
     - Highly recommended: write detailed section in recommendation note
     - Generally recommended: write basic info only

### 4.2 Recommendation Note Structure

Note file structure:

```markdown
---
keywords: [keyword1, keyword2, ...]
tags: ["llm-generated", "daily-paper-recommend"]
---

[Paper recommendation list...]
```

#### 4.2.1 Today's Overview (before paper list)

Add a "Today's Overview" section before the paper list:

```markdown
## 今日概览

今日推荐的{paper_count}篇论文主要聚焦于**{direction1}**、**{direction2}**和**{direction3}**等前沿方向。

- **总体趋势**：{summarize overall research trends}

- **质量分布**：今日推荐的论文评分在 {min_score}-{max_score} 之间，{overall quality assessment}。

- **研究热点**：
  - **{hotspot1}**：{brief description}
  - **{hotspot2}**：{brief description}
  - **{hotspot3}**：{brief description}

- **阅读建议**：{reading order suggestions}
```

**Notes**:
- Summarize based on top 10 papers' titles, abstracts, and scores
- Extract common research themes and trends
- Provide reasonable reading order suggestions

#### 4.2.2 Unified Format for All Papers

All papers sorted by score from high to low, using unified format:

```markdown
### [[Paper Title]]
- **作者**：[author list]
- **机构**：[institution name]
- **链接**：[arXiv](link) | [PDF](link)
- **来源**：[arXiv]
- **笔记**：[[existing note path]] or <<none>>

**一句话总结**：[one-sentence summary of core contribution]

**核心贡献/观点**：
- [contribution 1]
- [contribution 2]
- [contribution 3]

**关键结果**：[most important results from abstract]

---
```

**Notes**:
- Paper title uses wikilink format: `[[Paper Title]]`
- For top 3 papers, paper title links to detailed report
- For other papers, paper title serves as wikilink placeholder

#### 4.2.3 Top 3 Papers: Insert Images and Call Detailed Analysis

For top 3 papers (highest scoring):

**Step 0: Check if paper already has notes**
```bash
# Search in 50_Clippings/Papers/ directory for existing notes
# Search methods:
# 1. By paper ID (e.g., 2602.23351)
# 2. By paper title (fuzzy match)
# 3. By paper title keywords
```

**Step 1: Decide processing based on check results**

If note exists:
- Don't generate new detailed report
- Use existing note path as wikilink
- Reference existing note in "detailed report" field
- Check if images need extraction (if no images directory or empty)
  - If images needed: call `/obsidian-claude:paper-images`
  - If images exist: use existing images

If no note:
- Call `/obsidian-claude:paper-images` to extract images
- Call `/obsidian-claude:paper-analyze` to generate detailed report
- Add images and detailed report link in recommendation note

**Step 2: Insert images and links in recommendation note**

**If note exists**:
```markdown
### [[Existing Paper Title]]
- **作者**：[author list]
- **机构**：[institution name]
- **链接**：[arXiv](link) | [PDF](link)
- **来源**：[arXiv]
- **详细报告**：[[existing note path]]
- **笔记**：已有详细分析

**一句话总结**：[one-sentence summary]

![existing image|600](existing image path)

**核心贡献/观点**：
...
```

**If no note**:
```markdown
### [[Paper Title]]
- **作者**：[author list]
- **机构**：[institution name]
- **链接**：[arXiv](link) | [PDF](link)
- **来源**：[arXiv]
- **详细报告**：[[detailed report path]] (auto-generated)

**一句话总结**：[one-sentence summary]

![extracted image|600](image path)

**核心贡献/观点**：
...
```

**Image notes**:
- Image path: `50_Clippings/Papers/[domain]/images/[paper_id]_fig1.png`
- Width set to 600px
- Auto-extracted, no manual operation needed

**Detailed report notes**:
- Report path: `50_Clippings/Papers/[domain]/[paper_title].md`
- Paper title uses wikilink format: `[[Paper Title]]`, linked to detailed report
- Show wikilink in "detailed report" field for enhanced readability
- Detailed report auto-generated by `/obsidian-claude:paper-analyze`

## Step 5: Auto-link Keywords (Optional)

After generating recommendation note, auto-link keywords to existing notes:

```bash
# Step 1: Scan existing notes
cd "$SKILL_DIR"
python scripts/scan_existing_notes.py \
  --vault "$OBSIDIAN_VAULT_PATH" \
  --output existing_notes_index.json

# Step 2: Generate recommendation note (normal flow)
# ... use search_arxiv.py to search papers ...

# Step 3: Link keywords (new step)
python scripts/link_keywords.py \
  --index existing_notes_index.json \
  --input 50_Clippings/Papers/Daily/YYYY-MM-DD论文推荐.md \
  --output 50_Clippings/Papers/Daily/YYYY-MM-DD论文推荐_linked.md
```

**Notes**:
- Keyword linking script auto-skips frontmatter, headings, code blocks
- Filters common words (and, for, model, learning, etc.)
- Preserves existing wikilinks

# Important Rules

- **Expanded search scope**: search recent month + recent year trending papers
- **Comprehensive recommendation score**: combines relevance, recency, popularity, quality
- **Date-based filename**: keep `50_Clippings/Papers/Daily/YYYY-MM-DD论文推荐.md` format
- **Add today's overview**: add "## 今日概览" section at beginning summarizing main research directions, trends, quality distribution, hotspots, and reading suggestions
- **Sort by score**: all papers sorted by recommendation score high to low
- **Top 3 special processing**:
  - Paper title in wikilink format: `[[Paper Title]]`
  - Auto-extract first image and insert
  - Auto-call `/obsidian-claude:paper-analyze` to generate detailed report
  - Show wikilink association in "detailed report" field
- **Other papers**: basic info only, no images
- **Stay fast**: let user quickly understand daily recommendations
- **Avoid duplicates**: check previously recommended papers
- **Auto keyword linking**:
  - After generating recommendation note, auto-scan existing notes
  - Replace keywords in text (e.g., BLIP, CLIP) with wikilinks
  - Example: `BLIP` -> `[[BLIP]]`
  - Preserve existing wikilinks
  - Don't replace content in code blocks
  - Don't replace content already in wikilinks (avoid duplicates)

# Differences from Other Skills

## paper-recommend (this skill)
- **Purpose**: filter recommended papers from broad search, generate daily recommendation note
- **Search scope**: recent month + recent year trending/quality papers
- **Content**: recommendation list
  - Starts with "Today's Overview": summarizes main directions, trends, quality, hotspots, reading suggestions
  - All papers in unified format
  - Top 3 special processing:
    - Paper title in wikilink format: `[[Paper Title]]`
    - Auto-extract first image and insert
    - Auto-call `/obsidian-claude:paper-analyze` for detailed report
    - Show wikilink in "detailed report" field
- **Images**: top 3 auto-extract and insert first image; not all papers
- **Detailed reports**: top 3 auto-generated, others not
- **Usage**: user triggers manually each day
- **Note references**: if paper has existing note, write briefly and reference; if analysis needs historical notes, reference directly

## paper-analyze (deep analysis skill)
- **Purpose**: user actively views single paper, deep research
- **Use case**: papers user wants to read but AI hasn't organized
- **Content**: detailed deep analysis note
  - Contains all core info: research question, method overview, architecture, key innovations, experimental results, deep analysis, related paper comparison
  - **Rich with images**: all paper images should be used (architecture diagrams, method diagrams, experiment result figures, etc.)
- **Usage**: user actively calls `/obsidian-claude:paper-analyze [paper_id]` or paper title
- **Important**: whether papers are organized by paper-recommend or actively viewed by user, all should be richly illustrated

# Usage Instructions

When user inputs "start my day" or calls this skill, execute the following steps:

**Date parameter support**:
- No parameter: generate today's recommendation note
- With parameter (YYYY-MM-DD): generate recommendation note for specified date
  - Example: `/obsidian-claude:paper-recommend 2026-02-27`

## Auto-execution Flow

1. **Get target date**
   - No parameter: use current date (YYYY-MM-DD format)
   - With parameter: use specified date

2. **Scan existing notes to build index**
   ```bash
   # Scan existing paper notes in vault
   cd "$SKILL_DIR"
   python scripts/scan_existing_notes.py \
     --vault "$OBSIDIAN_VAULT_PATH" \
     --output existing_notes_index.json
   ```
   - Scan `50_Clippings/Papers/` directory
   - Extract note titles and tags
   - Build keyword-to-note-path mapping

3. **Search and filter arXiv papers**
   ```bash
   # Use Python script to search, parse, and filter arXiv papers
   # First cd to skill directory, then run script
   # If target date parameter given (e.g., 2026-02-21), pass to --target-date
   cd "$SKILL_DIR"
   python scripts/search_arxiv.py \
     --config "$OBSIDIAN_VAULT_PATH/90_System/Config/research_interests.yaml" \
     --output arxiv_filtered.json \
     --max-results 200 \
     --top-n 10 \
     --categories "cs.AI,cs.LG,cs.CL,cs.CV,cs.MM,cs.MA,cs.RO" \
     --target-date "{target_date}"  # if user specified a date, replace with actual date
   ```

4. **Read filtered results**
   - Read filtered results from `arxiv_filtered.json`
   - Get top 10 high-scoring papers
   - Each paper contains: ID, title, authors, abstract, scores, matched domains

5. **Generate recommendation note (with keyword linking)**
   - Create `50_Clippings/Papers/Daily/YYYY-MM-DD论文推荐.md` (using target date)
   - **Sort by score**: all papers by recommendation score high to low
   - **Top 3 special processing**:
     - Paper title in wikilink format: `[[Paper Title]]`
     - Insert actual extracted first image after "one-sentence summary"
     - Show wikilink in "detailed report" field
   - **Other papers**: basic info only, no images
   - **Auto keyword linking** (important!):
     - After generating note, scan text for keywords
     - Use `existing_notes_index.json` for matching
     - Replace keywords with wikilinks, e.g., `BLIP` -> `[[BLIP]]`
     - Preserve existing wikilinks
     - Don't replace content in code blocks

6. **Execute deep analysis for top 3 papers**
   ```bash
   # For each of the top 3 papers:

   # Step 1: Check if paper already has notes
   # Search in 50_Clippings/Papers/ directory
   # - By paper ID (e.g., 2602.23351)
   # - By paper title (fuzzy match)
   # - By paper title keywords (e.g., "Pragmatics", "Reporting Bias")

   # Step 2: Decide processing based on results
   if note_exists:
       # Don't generate new detailed report
       # Use existing note path
       # Only extract images (if none exist)
   else:
       # Extract first image
       /obsidian-claude:paper-images [paper_id]

       # Generate detailed analysis report
       /obsidian-claude:paper-analyze [paper_id]
   ```
   - **If note exists**:
     - Don't regenerate detailed report
     - Use existing note path as wikilink
     - Check if images needed (if no images directory or empty)
     - Reference existing note in "detailed report" field
   - **If no note**:
     - Extract first image and save to vault
     - Generate detailed paper analysis report
     - Add images and detailed report link in recommendation note

## Temp File Cleanup

- Temp XML and JSON files from search can be cleaned up
- After recommendation note saved to vault, temp files no longer needed

## Dependencies

- Python 3.x (for running search and filter scripts)
- PyYAML (for reading research interests config)
- Network connection (access arXiv API)
- `50_Clippings/Papers/` directory (for scanning existing notes and saving detailed reports)
- `/obsidian-claude:paper-images` skill (for extracting paper images)
- `/obsidian-claude:paper-analyze` skill (for generating detailed reports)

## Script Documentation

### search_arxiv.py

Located at `scripts/search_arxiv.py`, functions include:

1. **Search arXiv**: call arXiv API to get papers
2. **Parse XML**: extract paper info (ID, title, authors, abstract, etc.)
3. **Filter papers**: filter based on research interests config
4. **Calculate scores**: comprehensive relevance, recency, quality dimensions
5. **Output JSON**: save filtered results to `arxiv_filtered.json`

### scan_existing_notes.py

Located at `scripts/scan_existing_notes.py`, functions include:

1. **Scan notes directory**: scan all `.md` files under `50_Clippings/Papers/`
2. **Extract note info**:
   - File path
   - Filename
   - Frontmatter title field
   - Tags field
3. **Build index**: create keyword-to-note-path mapping
4. **Output JSON**: save index to `existing_notes_index.json`

**Usage**:
```bash
cd "$SKILL_DIR"
python scripts/scan_existing_notes.py \
  --vault "$OBSIDIAN_VAULT_PATH" \
  --output existing_notes_index.json
```

**Output format**:
```json
{
  "notes": [
    {
      "path": "50_Clippings/Papers/multimodal/BLIP_Bootstrapping-Language-Image-Pre-training.md",
      "filename": "BLIP_Bootstrapping-Language-Image-Pre-training.md",
      "title": "BLIP: Bootstrapping Language-Image Pre-training for Unified Vision-Language Understanding and Generation",
      "title_keywords": ["BLIP", "Bootstrapping", "Language-Image", "Pre-training", "Unified", "Vision-Language", "Understanding", "Generation"],
      "tags": ["Vision-Language-Pre-training", "Multimodal-Encoder-Decoder", "Bootstrapping", "Image-Captioning", "Image-Text-Retrieval", "VQA"]
    }
  ],
  "keyword_to_notes": {
    "blip": ["50_Clippings/Papers/multimodal/BLIP_Bootstrapping-Language-Image-Pre-training.md"],
    "bootstrapping": ["50_Clippings/Papers/multimodal/BLIP_Bootstrapping-Language-Image-Pre-training.md"],
    "vision-language": ["50_Clippings/Papers/multimodal/BLIP_Bootstrapping-Language-Image-Pre-training.md"]
  }
}
```

### link_keywords.py

Located at `scripts/link_keywords.py`, functions include:

1. **Read text**: read text content to process
2. **Read note index**: load note mapping from `existing_notes_index.json`
3. **Replace keywords**: find keywords in text, replace with wikilinks
   - Don't replace existing wikilinks (e.g., `[[BLIP]]`)
   - Don't replace content in code blocks
   - Matching rules:
     - Prefer matching complete title keywords
     - Then match tag keywords
     - Case-insensitive matching
     - Filter common words (and, for, model, learning, etc.)
     - Skip frontmatter and heading lines
4. **Output results**: output processed text

**Usage**:
```bash
# First cd to skill directory, then run script
cd "$SKILL_DIR"
python scripts/link_keywords.py \
  --index existing_notes_index.json \
  --input "input.txt" \
  --output "output.txt"
```

**Matching example**:
```
Original text:
"This paper uses BLIP and CLIP as baseline methods."

Processed:
"This paper uses [[BLIP]] and [[CLIP]] as baseline methods."
```

**Key features**:
- **Smart matching**: case-insensitive matching in Chinese context
- **Protect existing links**: don't replace existing wikilinks
- **Avoid code pollution**: don't replace content in code blocks and inline code
- **Path encoding**: use UTF-8 encoding for correct Chinese paths
- **Skip sensitive areas**: don't process frontmatter, heading lines, code blocks
