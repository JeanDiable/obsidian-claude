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

## 基本信息
- **论文ID**: arXiv:XXXX.XXXXX
- **作者**: [Author1, Author2, Author3]
- **机构**: [inferred from authors or paper]
- **发表日期**: YYYY-MM-DD
- **会议/期刊**: [inferred from categories]
- **链接**: [arXiv](link) | [PDF](link)
- **引用数**: [if available]

## 摘要

### 英文摘要
[Original English abstract]

### 中文翻译
[Fluent Chinese translation preserving academic terminology]

### 要点提炼
- **研究背景**: [该领域的发展现状与存在问题]
- **研究动机**: [为何需要此研究]
- **核心方法**: [一句话概括核心方法]
- **主要结果**: [最重要的实验结果]
- **研究意义**: [对该领域的贡献]

## 研究背景与动机

### 领域现状
[详细描述该领域的发展现状]

### 现有方法的局限性
[深入分析现有方法存在的问题]

### 研究动机
[阐述为何需要此研究]

## 研究问题

### 核心研究问题
[清晰、准确地描述核心问题]

## 方法概述

### 核心思想
[用通俗易懂的语言解释核心思想]

### 方法框架

#### 整体架构
[描述整体架构，包括主要组件及其关系]

**架构图选择原则**：
1. **优先使用论文原图** - 如论文PDF中有架构/流程/方法图表，直接插入
2. **仅在无原图时创建 Canvas** - 论文缺乏合适的图表时，使用 JSON Canvas

**方案一：插入论文原图（推荐）**
```
![Architecture|800](images/pageX_figY.pdf)

> 图1：[架构描述，包含各组件含义及其关系]
```
**注意**：图片文件名须与实际文件名一致（arXiv提取的图片通常为 `.pdf` 格式）

**方案二：创建 Canvas 架构图（论文无原图时）**
调用 `json-canvas` 技能创建 `.canvas` 文件，然后嵌入：
```
![[Paper_Title_Architecture.canvas|1200|400]]
```

#### 模块详述

**模块一：[模块名称]**
- **功能**: [主要功能]
- **输入**: [输入数据/信息]
- **输出**: [输出数据/信息]
- **处理流程**:
  1. [步骤1描述]
  2. [步骤2描述]
  3. [步骤3描述]
- **关键技术**: [关键技术或算法]
- **数学公式**: [如有重要公式]

**模块二：[模块名称]**
[类似格式]

## 实验结果

### 实验目标
[本实验验证的内容]

### 数据集

#### 数据集统计

| 数据集 | 样本数 | 特征维度 | 类别数 | 数据类型 |
|---------|---------|-------------|---------|-----------|
| Dataset1 | Xk | Y dim | Z classes | [type] |

### 实验设置

#### 基线方法
[列出所有对比基线并简要描述]

#### 评估指标
[列出所有评估指标并说明]

#### 实验环境

#### 超参数设置

### 主要结果

#### 主实验结果

| 方法 | 数据集1-指标1 | 数据集1-指标2 | 数据集2-指标1 | 平均排名 |
|--------|-------------------|-------------------|-------------------|----------|
| Baseline1 | X.X±Y.Y | X.X±Y.Y | X.X±Y.Y | N |
| **Ours** | **X.X±Y.Y** | **X.X±Y.Y** | **X.X±Y.Y** | **N** |

> 注：± 表示标准差，**加粗** 表示最优结果

#### 结果分析
[对主要结果的详细分析]

### 消融实验

#### 实验设计
[消融实验设计理由]

#### 结果与分析

### 实验结果图表
[插入论文实验结果图表]

![Experiment Results|800](images/results.pdf)

> 图2：[图表描述]

## 深度分析

### 研究价值评估

#### 理论贡献
- **贡献一**: [详细描述]
  - 创新点: [新理论/方法/视角]
  - 学术价值: [对学术界的价值]
  - 影响范围: [影响的研究领域]

#### 实际应用价值
- **应用一**: [场景描述]
  - 适用性: [适用程度]
  - 优势: [相比现有方案]
  - 潜在影响: [可能的影响]

#### 领域影响
- **短期**: [近期影响]
- **中期**: [中期影响]
- **长期**: [长期影响]

### 方法优势

#### 优势一：[名称]
- **描述**: [详细描述]
- **技术基础**: [技术基础]
- **实验验证**: [实验验证]
- **对比分析**: [对比分析]

### 局限性分析

#### 局限一：[名称]
- **描述**: [详细描述]
- **表现**: [实践中的表现]
- **根本原因**: [根本原因]
- **影响**: [对实际应用的影响]
- **可能的解决方案**: [如何缓解或解决]

### 适用性分析

#### 适用场景
- **场景一**: [描述]
  - 适用原因: [原因]
  - 预期效果: [预期效果]
  - 注意事项: [需要注意的事项]

#### 不适用场景
- **场景一**: [描述]
  - 不适用原因: [原因]
  - 替代方案: [建议的替代方案]

## 相关论文对比

### 对比选择标准
[选择这些论文进行对比的原因]

### [[Related Paper 1]] - [Title]

#### 基本信息
- **作者**: [作者]
- **发表时间**: [日期]
- **发表会议/期刊**: [发表会议/期刊]
- **核心方法**: [一句话总结]

#### 方法对比
| 维度 | 相关论文1 | 本文 |
|-----------|----------------|------------|
| 核心思想 | [描述] | [描述] |
| 技术路线 | [描述] | [描述] |
| 关键组件 | [描述] | [描述] |
| 创新程度 | [描述] | [描述] |

#### 性能对比
| 数据集 | 指标 | 相关论文1 | 本文 | 提升幅度 |
|---------|--------|----------------|------------|-------------|
| Dataset1 | Metric1 | X.X | Y.Y | +Z.Z% |

#### 关系分析
- **关系类型**: [改进/扩展/对比/跟进]
- **本文改进点**: [改进之处]
- **优势**: [本文优势]
- **不足**: [本文不足]
- **互补性**: [方法是否互补]

### [[Related Paper 2]] - [Title]
[类似格式]

### 对比总结
[所有对比论文的总结]

## 技术路线图

### 研究脉络
本文属于[技术路线]研究方向，其核心特征为：
- 特征一: [描述]
- 特征二: [描述]

### 发展历程
```
[Milestone 1] -> [Milestone 2] -> [Milestone 3] -> [This Paper] -> [Future]
```

### 路线图中的位置
- **继承**: [继承了哪些前人工作]
- **开启**: [为未来工作奠定了什么基础]

## 未来工作建议

### 作者建议的未来工作
1. **建议一**: [作者的建议]
   - 可行性: [是否可行]
   - 价值: [潜在价值]
   - 难度: [实现难度]

### 分析得出的未来方向
1. **方向一**: [描述]
   - 动机: [为何值得研究]
   - 可能方法: [可能的方法]
   - 预期成果: [可能的成果]
   - 挑战: [面临的挑战]

### 改进建议
1. **改进一**: [描述]
   - 当前问题: [现有问题]
   - 改进方案: [如何改进]
   - 预期效果: [预期效果]

## 总体评估

### 价值评分

#### 综合评分
**[X.X]/10** - [简要评分理由]

#### 分项评分

| 维度 | 评分 | 理由 |
|-----------|-------|-----------|
| 创新性 | [X]/10 | [详细理由] |
| 技术质量 | [X]/10 | [详细理由] |
| 实验充分性 | [X]/10 | [详细理由] |
| 写作质量 | [X]/10 | [详细理由] |
| 实用性 | [X]/10 | [详细理由] |

### 重点关注

#### 值得注意的技术要点

#### 需要深入理解的部分

## 个人笔记

%% 在此添加个人阅读笔记 %%

## 相关论文

### 直接相关
- [[Related Paper 1]] - [关系：改进/扩展/对比]
- [[Related Paper 2]] - [关系]

### 背景相关
- [[Background Paper 1]] - [关系]
- [[Background Paper 2]] - [关系]

### 后续工作
- [[Follow-up Paper 1]] - [关系]
- [[Follow-up Paper 2]] - [关系]

## 外部资源
[列出相关视频、博客、项目等]

> [!tip] 核心洞察
> [论文最重要的洞察，一句话核心思想总结]

> [!warning] 注意事项
> - [Note 1]
> - [Note 2]

> [!success] 推荐
> [评分与简要理由]
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
