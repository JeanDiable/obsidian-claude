# Obsidian Claude

A [Claude Code](https://claude.ai/code) plugin for personal knowledge management in [Obsidian](https://obsidian.md/) vaults. Provides 15 skills for organizing notes using the PARA method, building an interconnected wiki, conducting research, generating content, and daily planning.

## Prerequisites

- [Claude Code](https://claude.ai/code) CLI installed and authenticated
- [Obsidian](https://obsidian.md/) with a vault organized using the [PARA method](https://fortelabs.com/blog/para/)
- [Obsidian CLI](https://github.com/Yakitrak/obsidian-cli) installed and configured for your vault

## Installation

### Step 1: Add the marketplace

```bash
claude plugin marketplace add JeanDiable/obsidian-claude
```

### Step 2: Install the plugin

```bash
claude plugin install obsidian-claude
```

### Step 3: Configure your vault

After installation, update the `CLAUDE.md` in the plugin directory to match your vault's folder structure, metadata schema, and naming conventions.

### Updating

```bash
claude plugin update obsidian-claude
```

### Uninstalling

```bash
claude plugin uninstall obsidian-claude
```

## Skills

### Vault Organization

| Skill | Command | Description |
|-------|---------|-------------|
| **organize-para** | `/organize-para` | Batch organize PARA files — add metadata, create wikilinks, build connections |
| **organize-clippings** | `/organize-clippings` | Consolidate and categorize clippings with metadata, summaries, and wikilinks |
| **process-inbox** | `/process-inbox` | Route inbox notes to appropriate PARA folders with metadata |
| **archive** | `/archive` | Review projects, identify completed/stale ones, generate wrap-ups |

### Knowledge Building

| Skill | Command | Description |
|-------|---------|-------------|
| **wiki-builder** | `/wiki-builder` | Extract atomic concepts from vault files and build interconnected wiki entries |
| **vault-query** | `/vault-query` | Query the vault (PARA + Clippings + Wiki) and get answers with source citations |
| **spaced-review** | `/spaced-review` | Spaced repetition — randomly select clippings and generate Q&A pairs for review |

### Research

| Skill | Command | Description |
|-------|---------|-------------|
| **deep-research** | `/deep-research` | Deep domain research — search vault + web, create comprehensive research notes |
| **news-research** | `/news-research` | Multi-domain news research with cross-domain analysis and investment impact assessment |

### Content & Planning

| Skill | Command | Description |
|-------|---------|-------------|
| **daily-plan** | `/daily-plan` | Generate daily diary with yesterday's review, today's tasks, and diet plan |
| **concept-to-action** | `/concept-to-action` | Convert abstract concepts (psychology, investment, self-growth) into executable projects |
| **xiaohongshu** | `/xiaohongshu` | Xiaohongshu (Little Red Book) content generation — analyze style, write posts |
| **create-base** | `/create-base` | Create Obsidian Bases database views with filters and formulas |

### AI Trends

| Skill | Command | Description |
|-------|---------|-------------|
| **ai-newsletter** | `/ai-newsletter` | Curate and summarize AI newsletter content |
| **ai-product** | `/ai-product` | Discover trending AI products from ProductHunt, HN, GitHub, Reddit |

## Vault Structure

This plugin expects a vault organized with the following folder structure:

```
My_note/
  00_Inbox/        # Raw capture, unprocessed notes
  10_Diary/        # Daily notes (YYYY-MM-DD.md)
  20_Projects/     # Active projects
  30_Areas/        # Ongoing responsibilities
  40_Resources/    # Reference material
  50_Clippings/    # External content (articles, papers)
  60_Wiki/         # Atomic concept definitions
  70_Plans/        # Planning documents
  80_Archive/      # Completed/archived content
  90_System/       # Templates and config
```

Customize the folder paths in `CLAUDE.md` to match your vault.

## License

[MIT](LICENSE)

## Author

[JeanDiable](https://github.com/JeanDiable)
