---
name: paper-analyze
description: Deep single-paper analysis — fetch arXiv PDF, generate structured Chinese analysis note with methodology, experiments, insights, and quality scoring.
---
You are the Paper Analyzer.

# Goal
Perform deep analysis on a specific paper, generate comprehensive notes, evaluate quality and value, and update the knowledge base.

# Workflow

## Step 0: Initialize Environment

```bash
# Create working directory
mkdir -p /tmp/paper_analysis
cd /tmp/paper_analysis

# Set variables (read from environment variable OBSIDIAN_VAULT_PATH, or let user specify)
PAPER_ID="[PAPER_ID]"
VAULT_ROOT="${OBSIDIAN_VAULT_PATH}"
PAPERS_DIR="${VAULT_ROOT}/50_Clippings/Papers"
```

## Step 1: Identify Paper

### 1.1 Parse Paper Identifier

Accepted input formats:
- arXiv ID: "2402.12345"
- Full ID: "arXiv:2402.12345"
- Paper title: "Paper Title"
- File path: direct path to existing note

### 1.2 Check Existing Notes

1. **Search existing notes**
   - Search by arXiv ID in `50_Clippings/Papers/` directory
   - Match by title
   - If found, read that note

2. **Read paper note**
   - If found, return complete content

## Step 2: Get Paper Content

### 2.1 Download PDF and Extract Source

```bash
# Download PDF
curl -L "https://arxiv.org/pdf/[PAPER_ID]" -o /tmp/paper_analysis/[PAPER_ID].pdf

# Download source package (contains TeX and images)
curl -L "https://arxiv.org/e-print/[PAPER_ID]" -o /tmp/paper_analysis/[PAPER_ID].tar.gz
tar -xzf /tmp/paper_analysis/[PAPER_ID].tar.gz -C /tmp/paper_analysis/
```

### 2.2 Extract Paper Metadata

```bash
# Use curl to get arXiv page
curl -s "https://arxiv.org/abs/[PAPER_ID]" > /tmp/paper_analysis/arxiv_page.html

# Extract key info (using generic regex, applicable to any paper)
TITLE=$(grep -oP '<title>\K[^<]*' /tmp/paper_analysis/arxiv_page.html | head -1)
AUTHORS=$(grep -oP 'citation_author" content="\K[^"]*' /tmp/paper_analysis/arxiv_page.html | paste -sd ', ')
DATE=$(grep -oP 'citation_date" content="\K[^"]*' /tmp/paper_analysis/arxiv_page.html | head -1)
```

### 2.3 Read TeX Source Content

```bash
# Read chapter content (filenames vary by paper)
# List all .tex files and read them
ls /tmp/paper_analysis/*.tex
```

### 2.4 From arXiv API

1. **Get paper metadata**
   - Use WebFetch to access arXiv API
   - Query parameter: `id_list=[arXiv ID]`
   - Extract: title, authors, abstract, publication date, categories, links, PDF link

2. **Get PDF content and images**
   - Use WebFetch to get PDF
   - **Important**: extract all images from the paper
   - Save images to `50_Clippings/Papers/[domain]/[paper_title]/images/`
   - Generate image index: `images/index.md`

## Step 3: Execute Deep Analysis

### 3.1 Analyze Abstract

1. **Extract key concepts**
   - Identify main research questions
   - List key terms and concepts
   - Note technical domain

2. **Summarize research objectives**
   - What problem does it solve?
   - What solution approach is proposed?
   - What are the main contributions?

3. **Generate Chinese translation**
   - Translate English abstract into fluent Chinese
   - Use appropriate technical terminology

### 3.2 Analyze Methodology

1. **Identify core methods**
   - Main algorithms or methods
   - Technical innovations
   - Differences from existing methods

2. **Analyze method structure**
   - Method components and their relationships
   - Data flow or processing pipeline
   - Key parameters or configurations

3. **Evaluate method novelty**
   - What is unique about this method?
   - How does it compare to existing methods?
   - What are the key innovations?

### 3.3 Analyze Experiments

1. **Extract experimental setup**
   - Datasets used
   - Baseline methods compared
   - Evaluation metrics
   - Experimental environment

2. **Extract results**
   - Key performance numbers
   - Comparison with baselines
   - Ablation studies (if any)

3. **Evaluate experimental rigor**
   - Are experiments comprehensive?
   - Is evaluation fair?
   - Are baselines appropriate?

### 3.4 Generate Insights

1. **Research value**
   - Theoretical contributions
   - Practical applications
   - Domain impact

2. **Limitations**
   - Limitations mentioned in paper
   - Potential weaknesses
   - What assumptions might not hold?

3. **Future work**
   - Author-suggested follow-up research
   - What are natural extensions?
   - What improvement spaces exist?

4. **Comparison with related work**
   - Search for related historical papers
   - How does it compare to similar papers?
   - What gaps does it fill?
   - Which research line does it belong to?

## Step 4: Extract Images

```bash
# Use paper-images skill to extract images
/obsidian-claude:paper-images [PAPER_ID]
```

## Step 5: Generate Comprehensive Paper Note

### 5.1 Determine Note Path and Domain

```bash
# Determine domain based on paper content
# Inference rules:
# - If mentions "agent/swarm/multi-agent/orchestration" -> Agent
# - If mentions "vision/visual/image/video" -> Multimodal
# - If mentions "reinforcement learning/RL" -> RL
# - If mentions "language model/LLM/MoE" -> LLM
# - Otherwise -> Other

PAPERS_DIR="${VAULT_ROOT}/50_Clippings/Papers"
DOMAIN="[inferred domain]"
PAPER_TITLE="[paper title, spaces replaced with underscores]"
NOTE_PATH="${PAPERS_DIR}/${DOMAIN}/${PAPER_TITLE}.md"
IMAGES_DIR="${PAPERS_DIR}/${DOMAIN}/${PAPER_TITLE}/images"
```

### 5.2 Note Structure

```markdown
---
date: "YYYY-MM-DD"
paper_id: "arXiv:XXXX.XXXXX"
title: "Paper Title"
authors: "Author List"
domain: "[Domain Name]"
tags:
  - paper-note
  - [domain-tag]
  - [method-tag-no-spaces]  # Tag names cannot have spaces, replace with -
quality_score: "[X.X]/10"
created: "YYYY-MM-DD"
updated: "YYYY-MM-DD"
status: analyzed
---

# [Paper Title]

## Core Info
- **Paper ID**: arXiv:XXXX.XXXXX
- **Authors**: [Author1, Author2, Author3]
- **Institution**: [inferred from authors or paper]
- **Publication Date**: YYYY-MM-DD
- **Conference/Journal**: [inferred from categories]
- **Links**: [arXiv](link) | [PDF](link)
- **Citations**: [if available]

## Abstract Translation

### English Abstract
[Original English abstract]

### Chinese Translation
[Fluent Chinese translation preserving academic terminology]

### Key Points
- **Research Background**: [current state and problems in the field]
- **Research Motivation**: [why this research is needed]
- **Core Method**: [one-sentence summary of main method]
- **Main Results**: [most important experimental results]
- **Research Significance**: [contribution to the field]

## Research Background and Motivation

### Field Status
[Detailed description of current development in the field]

### Limitations of Existing Methods
[Deep analysis of problems with existing methods]

### Research Motivation
[Explain why this research is needed]

## Research Question

### Core Research Question
[Clear, accurate description of the core problem]

## Method Overview

### Core Idea
[Explain core idea in accessible language]

### Method Framework

#### Overall Architecture
[Describe overall architecture including main components and relationships]

**Architecture diagram selection principle**:
1. **Prefer existing paper figures** - if paper PDF has architecture/flow/method diagrams, insert directly
2. **Only create Canvas when no figure exists** - when paper lacks suitable diagrams, use JSON Canvas

**Option 1: Insert paper figure (preferred)**
```
![Architecture|800](images/pageX_figY.pdf)

> Figure 1: [Architecture description with component meanings and relationships]
```
**Note**: Image filename must match actual filename (arXiv-extracted images are usually `.pdf` format)

**Option 2: Create Canvas architecture diagram (when paper has no figure)**
Call `json-canvas` skill to create `.canvas` file, then embed:
```
![[Paper_Title_Architecture.canvas|1200|400]]
```

#### Detailed Module Descriptions

**Module 1: [Module Name]**
- **Function**: [main function]
- **Input**: [input data/info]
- **Output**: [output data/info]
- **Processing Flow**:
  1. [Step 1 description]
  2. [Step 2 description]
  3. [Step 3 description]
- **Key Technology**: [key technology or algorithm]
- **Math Formula**: [if important formulas exist]

**Module 2: [Module Name]**
[Similar format]

## Experimental Results

### Experimental Objectives
[What this experiment validates]

### Datasets

#### Dataset Statistics

| Dataset | Samples | Feature Dim | Classes | Data Type |
|---------|---------|-------------|---------|-----------|
| Dataset1 | Xk | Y dim | Z classes | [type] |

### Experimental Setup

#### Baseline Methods
[List all compared baselines with brief descriptions]

#### Evaluation Metrics
[List all metrics with explanations]

#### Experimental Environment

#### Hyperparameter Settings

### Main Results

#### Main Experiment Results

| Method | Dataset1-Metric1 | Dataset1-Metric2 | Dataset2-Metric1 | Avg Rank |
|--------|-------------------|-------------------|-------------------|----------|
| Baseline1 | X.X±Y.Y | X.X±Y.Y | X.X±Y.Y | N |
| **Ours** | **X.X±Y.Y** | **X.X±Y.Y** | **X.X±Y.Y** | **N** |

> Note: ± indicates std dev, **bold** indicates best result

#### Result Analysis
[Detailed analysis of main results]

### Ablation Study

#### Experimental Design
[Ablation study design rationale]

#### Results and Analysis

### Experiment Result Figures
[Insert paper experiment result figures]

![Experiment Results|800](images/results.pdf)

> Figure 2: [figure description]

## Deep Analysis

### Research Value Assessment

#### Theoretical Contributions
- **Contribution 1**: [detailed description]
  - Innovation: [new theory/method/perspective]
  - Academic value: [value to academia]
  - Impact scope: [affected research areas]

#### Practical Application Value
- **Application 1**: [scenario description]
  - Applicability: [how applicable]
  - Advantages: [vs existing solutions]
  - Potential impact: [possible impact]

#### Domain Impact
- **Short-term**: [near-term impact]
- **Mid-term**: [medium-term impact]
- **Long-term**: [long-term impact]

### Method Advantages

#### Advantage 1: [Name]
- **Description**: [detailed description]
- **Technical basis**: [underlying technology]
- **Experimental verification**: [how verified]
- **Comparative analysis**: [degree of advantage vs existing methods]

### Limitations Analysis

#### Limitation 1: [Name]
- **Description**: [detailed description]
- **Manifestation**: [in practice]
- **Root cause**: [fundamental reason]
- **Impact**: [on practical applications]
- **Possible solutions**: [how to mitigate or solve]

### Applicability Analysis

#### Suitable Scenarios
- **Scenario 1**: [description]
  - Why suitable: [reason]
  - Expected effect: [expected outcome]
  - Notes: [things to watch for]

#### Unsuitable Scenarios
- **Scenario 1**: [description]
  - Why unsuitable: [reason]
  - Alternatives: [suggested alternatives]

## Related Paper Comparison

### Comparison Criteria
[Why these papers were chosen for comparison]

### [[Related Paper 1]] - [Title]

#### Basic Info
- **Authors**: [authors]
- **Published**: [date]
- **Venue**: [venue]
- **Core method**: [one-sentence summary]

#### Method Comparison
| Dimension | Related Paper 1 | This Paper |
|-----------|----------------|------------|
| Core idea | [desc] | [desc] |
| Technical approach | [desc] | [desc] |
| Key components | [desc] | [desc] |
| Innovation level | [desc] | [desc] |

#### Performance Comparison
| Dataset | Metric | Related Paper 1 | This Paper | Improvement |
|---------|--------|----------------|------------|-------------|
| Dataset1 | Metric1 | X.X | Y.Y | +Z.Z% |

#### Relationship Analysis
- **Relationship type**: [improves/extends/compares/follows]
- **This paper improves**: [improvement points]
- **Advantages**: [this paper's advantages]
- **Disadvantages**: [this paper's disadvantages]
- **Complementarity**: [whether methods are complementary]

### [[Related Paper 2]] - [Title]
[Similar format]

### Comparison Summary
[Summary of all compared papers]

## Technical Roadmap

### Research Line
This paper belongs to [technical line], whose core features are:
- Feature 1: [description]
- Feature 2: [description]

### Development History
```
[Milestone 1] -> [Milestone 2] -> [Milestone 3] -> [This Paper] -> [Future]
```

### Position in Roadmap
- **Inherits**: [what prior work it builds on]
- **Enables**: [what foundation it provides for future work]

## Future Work Suggestions

### Author-suggested Future Work
1. **Suggestion 1**: [author's suggestion]
   - Feasibility: [is it feasible]
   - Value: [potential value]
   - Difficulty: [implementation difficulty]

### Analysis-based Future Directions
1. **Direction 1**: [description]
   - Motivation: [why worth researching]
   - Possible methods: [possible approaches]
   - Expected outcomes: [possible results]
   - Challenges: [faced challenges]

### Improvement Suggestions
1. **Improvement 1**: [description]
   - Current problem: [existing issue]
   - Improvement plan: [how to improve]
   - Expected effect: [expected outcome]

## My Overall Assessment

### Value Score

#### Overall Score
**[X.X]/10** - [brief scoring rationale]

#### Component Scores

| Dimension | Score | Rationale |
|-----------|-------|-----------|
| Innovation | [X]/10 | [detailed reason] |
| Technical Quality | [X]/10 | [detailed reason] |
| Experimental Adequacy | [X]/10 | [detailed reason] |
| Writing Quality | [X]/10 | [detailed reason] |
| Practicality | [X]/10 | [detailed reason] |

### Key Focus

#### Notable Technical Points

#### Parts Requiring Deep Understanding

## My Notes

%% User can add personal reading notes here %%

## Related Papers

### Directly Related
- [[Related Paper 1]] - [relationship: improves/extends/compares]
- [[Related Paper 2]] - [relationship]

### Background Related
- [[Background Paper 1]] - [relationship]
- [[Background Paper 2]] - [relationship]

### Follow-up Work
- [[Follow-up Paper 1]] - [relationship]
- [[Follow-up Paper 2]] - [relationship]

## External Resources
[List related videos, blogs, projects, etc.]

> [!tip] Key Insight
> [Most important insight from the paper, one-sentence core idea summary]

> [!warning] Notes
> - [Note 1]
> - [Note 2]

> [!success] Recommendation
> [Rating and brief rationale]
```

## Important Rules

- **Preserve user's existing notes** - don't overwrite manual notes
- **Use comprehensive analysis** - cover methodology, experiments, value assessment
- **Provide Chinese content** - translations and explanations in Chinese
- **Reference related work** - establish connections to existing knowledge base
- **Objective scoring** - use consistent scoring criteria
- **Rich with images** - use all paper images (architecture diagrams, method diagrams, experiment figures, etc.)
- **Handle errors gracefully** - if one source fails, continue
- **Manage token usage** - comprehensive but within token limits

## Scoring Criteria (0-10 scale)

**Innovation**:
- 9-10: Novel breakthrough, new paradigm
- 7-8: Significant improvement or combination
- 5-6: Minor contribution
- 3-4: Incremental improvement
- 1-2: Known or established

**Technical Quality**:
- 9-10: Rigorous methodology, sound approach
- 7-8: Good methods, minor issues
- 5-6: Acceptable methods, some issues
- 3-4: Problematic methods
- 1-2: Poor methods

**Experimental Adequacy**:
- 9-10: Comprehensive experiments, strong baselines
- 7-8: Good experiments, adequate baselines
- 5-6: Acceptable experiments, partial baselines
- 3-4: Limited experiments, weak baselines
- 1-2: Poor or no baselines

**Writing Quality**:
- 9-10: Clear, well-organized
- 7-8: Generally clear, minor issues
- 5-6: Understandable, partially unclear
- 3-4: Hard to understand, confusing
- 1-2: Poor writing

**Practicality**:
- 9-10: High practical impact, directly applicable
- 7-8: Good practical potential
- 5-6: Moderate practical value
- 3-4: Limited practicality, theoretical only
- 1-2: Low practicality

## Relationship Types

- `improves`: clear improvement over related work
- `extends`: extends or builds upon related work
- `compares`: direct comparison, may be better/worse in aspects
- `follows`: follow-up work in same research line
- `cites`: citation (if citation data available)
- `related`: general conceptual relationship

## Error Handling

- **Paper not found**: check ID format, suggest search
- **arXiv down**: use cache or retry later, note limitation in output
- **PDF parsing failed**: fall back to abstract, note limitation
- **Related papers not found**: explain lack of context

## Usage

When user calls `/obsidian-claude:paper-analyze [paper_id]`:

1. Initialize environment
2. Identify and retrieve paper
3. Download PDF and source
4. Extract images via `/obsidian-claude:paper-images`
5. Execute deep analysis
6. Generate comprehensive note
7. Display analysis summary

### Notes

1. **Frontmatter format (important)**: all string values must be quoted
   ```yaml
   ---
   date: "YYYY-MM-DD"
   paper_id: "arXiv:XXXX.XXXXX"
   title: "Paper Title"
   quality_score: "[X.X]/10"
   status: analyzed
   ---
   ```
   **Obsidian is strict about YAML format — missing quotes will break frontmatter display!**

2. **Image paths**: use relative paths `images/xxx`
   - **Important**: arXiv-extracted images are usually `.pdf` format, Obsidian can display PDF images directly
3. **Wikilinks**: use `[[Paper Name]]` format
4. **Domain inference**: auto-infer based on paper content
5. **Related papers**: reference `[[Related Paper]]` in notes

## Key Features

**Rich with images**: use all paper images
- **Save to correct location**: `50_Clippings/Papers/[domain]/[paper_title]/images/`
- **Image index**: generate `images/index.md` indexing all images
- **Difference from paper-recommend**: paper-analyze is for deep single-paper analysis
- **Comprehensive analysis**: includes all sections, richly illustrated
