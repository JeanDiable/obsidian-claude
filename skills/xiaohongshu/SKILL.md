---
name: xiaohongshu
description: Xiaohongshu content generation — analyze style, write posts, polish drafts.
---

# Xiaohongshu (小红书)

Content generation for Xiaohongshu with three sub-commands: style analysis, writing, and polishing.

## Vault Path
~/Library/Mobile Documents/iCloud~md~obsidian/Documents/My_note

## Sub-Commands

### `/xiaohongshu style`

Analyze existing posts and generate/update style guide.

1. Read all .md files in `20_Projects/小红书/`
2. Analyze patterns:
   - Typical post length (character count)
   - Tone and voice (formal/casual, humorous/serious)
   - Common structure (hook → body → CTA?)
   - Emoji usage patterns
   - Hashtag conventions
   - Topic categories
   - Opening and closing patterns
3. Generate/update `90_System/Style/xiaohongshu_style.md`:
   ```markdown
   ---
   title: "小红书文风指南"
   created: YYYY-MM-DD
   modified: YYYY-MM-DD
   tags: [xiaohongshu, style-guide]
   description: "Style guide derived from existing posts"
   ---

   # 小红书文风指南

   ## 数据概览
   - 分析帖子数: N
   - 平均长度: X 字
   - 常见主题: ...

   ## 文风特征
   ### 语气
   [Formal/casual, direct/indirect, etc.]

   ### 结构模式
   [Typical post structure]

   ### 用词习惯
   [Common phrases, transition words, etc.]

   ### Emoji 使用
   [Patterns of emoji usage]

   ## 写作规则
   1. Rule derived from analysis
   2. Rule derived from analysis
   ...
   ```

### `/xiaohongshu write <topic>`

Generate a new post.

1. Read style guide from `90_System/Style/xiaohongshu_style.md`
2. If topic is technical:
   - Add critical, uncommon analytical angles
   - Include contrarian or non-obvious insights
   - Avoid generic takes that anyone could write
3. Generate post matching:
   - Length similar to existing posts
   - Voice and tone from style guide
   - Structure patterns from style guide
4. Save to `20_Projects/小红书/YYYY-MM-DD_<topic>.md`
5. Present to user for review before saving

### `/xiaohongshu polish`

Refine an existing draft.

1. Read the draft (user specifies file or provides text)
2. Read style guide
3. Improve while preserving the author's voice:
   - Fix awkward phrasing
   - Improve flow and structure
   - For technical content: add critical thinking angles
   - Match style guide patterns
4. Present both original and polished version for comparison
5. Save polished version (user confirms)

## Important Rules
- ALWAYS read the style guide before writing or polishing
- Preserve the author's authentic voice — enhance, don't replace
- Technical posts need non-obvious insights, not just summaries
- Match the typical length of existing posts (don't write significantly longer or shorter)
- All output in Chinese
