---
name: spaced-review
description: Spaced repetition — randomly select clippings, generate Q&A pairs for knowledge review.
---

# Review

Generate Q&A pairs from random categorized clippings for spaced repetition review.

## Vault Path
~/Library/Mobile Documents/iCloud~md~obsidian/Documents/My_note

## Input
Optional: `/review N` where N is the number of clippings to review (default: 5)

## Workflow

### Step 1: Select Random Clippings

1. List all .md files in `50_Clippings/` subfolders (only categorized ones, not root)
2. Randomly select N files
3. Read each selected file

### Step 2: Generate Q&A

For each selected clipping:

1. Analyze the content and identify 2-4 key knowledge points
2. Generate questions that test understanding (not just recall):
   - Conceptual: "Why does X work this way?"
   - Application: "How would you apply X in scenario Y?"
   - Analysis: "What are the tradeoffs between X and Y?"
   - Synthesis: "How does X relate to Y?"

3. Generate detailed answers with:
   - Direct answer to the question
   - Key knowledge points summarized
   - Links to source material and related wiki entries

### Step 3: Create Output Files

**Questions file**: `50_Clippings/Review_Questions_YYYY-MM-DD.md`
```markdown
---
title: "Review Questions YYYY-MM-DD"
created: YYYY-MM-DD
modified: YYYY-MM-DD
tags: [review, spaced-repetition]
description: "Review questions generated from N random clippings"
---

# Review Questions — YYYY-MM-DD

## From: [[ClippingTitle1]]
1. Question 1?
2. Question 2?

## From: [[ClippingTitle2]]
1. Question 3?
2. Question 4?

...
```

**Answers file**: `50_Clippings/Review_Answers_YYYY-MM-DD.md`
```markdown
---
title: "Review Answers YYYY-MM-DD"
created: YYYY-MM-DD
modified: YYYY-MM-DD
tags: [review, spaced-repetition]
description: "Answers and knowledge summaries for review questions"
---

# Review Answers — YYYY-MM-DD

## From: [[ClippingTitle1]]
### Q1: Question 1?
**Answer**: ...
**Key Points**:
- Point 1
- Point 2
**Related**: [[WikiEntry1]], [[WikiEntry2]]

### Q2: Question 2?
...
```

## Important Rules
- Questions should test UNDERSTANDING, not just memorization
- Answers should include enough context to be a learning resource
- Always link to source clippings and relevant wiki entries
- Vary question types across conceptual, application, analysis, synthesis
- Match language to the clipping content language
