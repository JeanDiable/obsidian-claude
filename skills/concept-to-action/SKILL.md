---
name: concept-to-action
description: Convert abstract concepts (psychology, sociology, investment, self-growth) into executable projects with goals and metrics.
---

# Actionize

Transform abstract knowledge into concrete, executable project plans.

## Vault Path
~/Library/Mobile Documents/iCloud~md~obsidian/Documents/My_note

## Input
User provides a file path or topic: `/actionize <path-or-topic>`

## Workflow

### Step 1: Analyze Source Material

1. If path provided: read the file
2. If topic provided: use `obsidian search query="<topic>"` to find relevant files in 50_Clippings and PARA
3. Extract the core ideas, principles, and actionable insights
4. Search vault for user's background context (existing projects, areas, goals)

### Step 2: Design Executable Project

Present a structured project plan to the user:

```markdown
## Project Proposal: <Name>

### Goal
[Specific, measurable goal derived from the abstract concept]

### Background
[How this connects to your existing knowledge and projects]
Links: [[SourceClipping]], [[RelatedArea]]

### Phases
#### Phase 1: Foundation (Week 1-2)
- [ ] Action item 1
- [ ] Action item 2
**Success metric**: [Quantifiable indicator]

#### Phase 2: Development (Week 3-6)
- [ ] Action item 3
- [ ] Action item 4
**Success metric**: [Quantifiable indicator]

#### Phase 3: Integration (Week 7-8)
- [ ] Action item 5
**Success metric**: [Quantifiable indicator]

### Quantified Success Indicators
- Metric 1: Target value by date
- Metric 2: Target value by date
- Overall: [How to know the project succeeded]

### Resources Needed
- Time: X hours/week
- Tools: ...
- Knowledge: ...
```

### Step 3: Save & Connect

After user approval:

1. Save as `20_Projects/<ProjectName>.md` with full frontmatter
2. Link back to source clippings/notes
3. Link to relevant wiki entries
4. Update source file's Related section

## Important Rules
- Goals MUST be specific and measurable — no vague aspirations
- Every phase needs a quantified success metric
- Action items must be concrete enough to start immediately
- Connect to existing vault knowledge (don't create in isolation)
- Consider user's available time and existing commitments
- Domains: psychology, sociology, investment, finance, self-growth, personal development
