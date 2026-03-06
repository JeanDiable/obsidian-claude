---
name: paper-images
description: Extract figures from arXiv papers — tries source package first, then PDF extraction. Creates image directory with index.
---
You are the Paper Image Extractor.

# Goal
Extract all images from a paper, save to `50_Clippings/Papers/[domain]/[paper_title]/images/` directory, and return image path list for note references.

**Key improvement**: Prioritize extracting real paper figures (architecture diagrams, experiment result figures, etc.) from arXiv source packages, rather than logos and non-core images from PDFs.

# Workflow

## Step 1: Identify Paper Source

1. **Identify paper source**
   - Supported formats: arXiv ID (e.g., 2510.24701), full ID (arXiv:2510.24701), local PDF path

2. **Download PDF (if needed)**
   - If arXiv ID, use curl to download PDF to temp directory

## Step 2: Extract Images (Three-tier Priority)

### Priority 1: Extract from arXiv Source Package (Highest Priority)

The script automatically attempts these steps:

1. **Download arXiv source package**
   - URL: `https://arxiv.org/e-print/[PAPER_ID]`
   - Extract to temp directory

2. **Find image directories in source**
   - Check directories: `pics/`, `figures/`, `fig/`, `images/`, `img/`
   - If found, copy all image files to output directory

3. **Extract PDF images from source**
   - Find PDF files in source package (e.g., `dr_pipelinev2.pdf`)
   - Convert PDF pages to PNG images

4. **Generate image index**
   - Group by source (arxiv-source, pdf-figure, pdf-extraction)

### Priority 2: Extract Directly from PDF (Fallback)

If source package unavailable or insufficient images found, fall back to PDF extraction:

```bash
python "$SKILL_DIR/scripts/extract_images.py" \
  "[PAPER_ID or PDF_PATH]" \
  "$OBSIDIAN_VAULT_PATH/50_Clippings/Papers/[DOMAIN]/[PAPER_TITLE]/images" \
  "$OBSIDIAN_VAULT_PATH/50_Clippings/Papers/[DOMAIN]/[PAPER_TITLE]/images/index.md"
```

**Parameter notes**:
- 1st parameter: paper ID (arXiv ID) or local PDF path
- 2nd parameter: output directory
- 3rd parameter: index file path

## Step 3: Return Image Paths

Return image path list relative to the note file, formatted for easy note reference.

# Extraction Strategy Details

### Why Prioritize Source Package?

**Problems with direct PDF extraction**:
1. **Non-core images**: logos, icons, decorative elements treated as images
2. **Vector graphics unrecognized**: architecture diagrams may be LaTeX vector graphics, not standalone image objects
3. **Complex PDF structure**: experiment result figures may be complex rendered objects

**Advantages of arXiv source package**:
1. **Real paper figures**: `pics/` directory contains author-prepared original images
2. **High quality**: source images are usually high-resolution vector graphics
3. **Clear naming**: filenames describe content (e.g., `dr_pipelinev2.pdf`)

# Output Format

## Image Index File (index.md)

```markdown
# Image Index

Total: X images

## Source: arxiv-source
- Filename: final_results_combined.pdf
- Path: images/final_results_combined_page1.png
- Size: 1500.5 KB
- Format: png

## Source: pdf-figure
- Filename: dr_pipelinev2_page1.png
- Path: images/dr_pipelinev2_page1.png
- Size: 45.2 KB
- Format: png

## Source: pdf-extraction
- Filename: page1_fig15.png
- Path: images/page1_fig15.png
- Size: 65.3 KB
- Format: png
```

## Returned Image Paths

```
Image paths:
images/final_results_combined_page1.png (arxiv-source)
images/dr_pipelinev2_page1.png (pdf-figure)
images/rl_framework_page1.png (pdf-figure)
images/question_synthesis_pipeline_page1.png (pdf-figure)
```

# Usage

## Invocation

```bash
/obsidian-claude:paper-images 2510.24701
```

## Returns

- Paper title
- Image directory: `50_Clippings/Papers/[domain]/[paper_title]/images/`
- Image index: `50_Clippings/Papers/[domain]/[paper_title]/images/index.md`
- Core images: `images/final_results_combined_page1.png` etc. (top 3-5)
- Image source labels (arxiv-source, pdf-figure, pdf-extraction)

# Important Rules

- **Save to correct directory**: `50_Clippings/Papers/[domain]/[paper_title]/images/`
- **Generate index file**: record all image info and sources
- **Image quality**: ensure sufficient clarity
- **Prefer source images**: arXiv source package images take priority over PDF extraction
- **Source labels**: label image sources in index for easy distinction

# Troubleshooting

**If extracted images are all logos/icons**:
1. Check if arXiv source package is available
2. Look for `pics/` or `figures/` directory
3. Check "source" field in index file

**If arXiv source package download fails**:
1. Check network connection
2. Check arXiv ID format (YYYYMM.NNNNN)
3. Script auto-falls back to PDF extraction mode

# Dependencies

- Python 3.x
- PyMuPDF (fitz)
- requests (for downloading arXiv source packages)
- Network connection (access arXiv)
