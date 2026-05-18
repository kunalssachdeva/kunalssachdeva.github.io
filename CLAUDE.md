# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Academic website for Kunal Sachdeva, Assistant Professor of Finance at University of Michigan Ross School of Business. Built with Hugo static site generator using the PaperMod theme, hosted on GitHub Pages at https://kunalssachdeva.github.io/ (custom domain via `static/CNAME`).

## Build Commands

```bash
# Install Hugo (first time only)
brew install hugo

# Local development server with live reload
hugo server

# Production build
hugo --gc --minify

# Build with draft content
hugo server -D
```

## Content Layout

All content lives in single Markdown files, not per-item folders. Edit these files directly to add new entries.

```
content/
├── research.md         # All papers (published, accepted, R&R, working) on one page → /research/
├── discussions.md      # All discussion slides on one page → /discussions/
└── data/
    └── _index.md       # Code & data resources → /data/

static/
├── CNAME               # Custom domain config
├── cv.pdf              # Linked from nav
├── profile.jpg         # Homepage portrait
├── files/slides/       # Discussion slide PDFs (naming convention: YYYY-MM-keyword.pdf)
└── images/papers/      # Per-paper SVG illustrations referenced from research.md

assets/css/extended/
├── michigan-theme.css
├── research-page.css   # Styling for the research and discussions pages
└── accessibility.css

layouts/
├── partials/
│   ├── index_profile.html    # Homepage profile/bio block (subtitle comes from config.yml)
│   └── head.html             # Modified for SEO meta tags
└── _default/
    └── baseof.html           # Base template with accessibility features
```

The homepage bio is rendered from `params.profileMode.subtitle` in `config.yml`, not from a content file.

## Adding Content

### New paper
Edit `content/research.md`. Each entry follows this pattern:

```markdown
### Paper Title
**[Author One](https://author1.com/), Kunal Sachdeva, [Author Three](https://author3.com/)**

*Journal Name* (Accepted)   <!-- or "*R&R, Journal Name*" or "*Journal Name* vol (year): pages" -->

![Alt Text](/images/papers/slug.svg)

One- or two-sentence description.

**Awards:** Optional awards line.

<div class="paper-links">
<a href="https://...">Paper</a>
<details>
<summary>Cite</summary>

```bibtex
@article{...}
```
</details>
</div>

---
```

Use the author's own academic page when linking — do NOT self-link "Kunal Sachdeva" back to the homepage.

The research page has two sections: "Published & Accepted Papers" (sorted with newest acceptance first) and "Working Papers" (R&Rs first, then plain working papers).

### New discussion
Edit `content/discussions.md`. Entries are grouped by year (newest first), with the venue and date formatted as `*Conference Name* · Month D, YYYY`. Author links point to faculty pages for professors; PhD students/job-market candidates are listed without a link. Save slides under `static/files/slides/YYYY-MM-keyword.pdf` to match the existing naming convention.

### Code & Data
Edit `content/data/_index.md` to add a project repo or replication package.

## Key Configuration

- **config.yml**: Site metadata, navigation menu, homepage bio (`params.profileMode.subtitle`), SEO description and keywords, social links.
- **Theme**: PaperMod (git submodule under `themes/PaperMod/`).
- **Custom domain**: www.kunalsachdeva.com via `static/CNAME`.

## Deployment

Automatic via GitHub Actions on push to `main`. Workflow at `.github/workflows/hugo.yml`. The `gh-pages` build output is published from the `public/` directory (gitignored locally).

## Accessibility & SEO

- Skip-to-content link, WCAG-compliant focus indicators, reduced-motion and high-contrast support (`assets/css/extended/accessibility.css`).
- Sitemap, canonical URLs, Open Graph, Twitter cards (theme defaults).
- Note: there is currently NO Google Scholar / Highwire Press citation metadata, because all papers live on a single listing page rather than per-paper pages. If Scholar indexing is desired in the future, restructure papers into `content/papers/<slug>/index.md` and re-add a `scholar_meta.html` partial gated on `eq .Section "papers"`.
