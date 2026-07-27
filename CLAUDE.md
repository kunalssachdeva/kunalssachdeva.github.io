# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Academic website for Kunal Sachdeva, Assistant Professor of Finance at University of Michigan Ross School of Business. Built with Hugo static site generator using the PaperMod theme, hosted on GitHub Pages at https://kunalssachdeva.github.io/.

There is **no custom domain in effect**. `static/CNAME` names `www.kunalsachdeva.com`, but that domain's DNS points at Google (`ghs.googlehosted.com`) where it serves an older Google Sites version of this site, and the GitHub Pages API reports `cname: null`. Under `build_type: workflow` GitHub reads the domain from repo settings and ignores a CNAME file in the build artifact, so that file currently does nothing. Do not register the domain in Pages settings until DNS is repointed, or the existing Google Site breaks.

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
content/                # these three files are the entire site
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

layouts/                # every file here overrides themes/PaperMod/<same path>
├── partials/
│   ├── index_profile.html    # Homepage profile/bio block (subtitle comes from config.yml); name is the h1
│   ├── head.html             # SEO meta tags + the site-wide og:image / twitter:image
│   ├── header.html           # Skip link, nav, Ross logo
│   ├── footer.html           # Scroll script (skip link deliberately excluded from it)
│   └── ...
└── _default/
    ├── baseof.html           # Base template; carries the #main-content skip target
    ├── list.html, single.html, _markup/render-link.html
    └── 404.html

An override that no longer differs meaningfully from the theme is dead weight and
blocks theme updates — delete it rather than carrying it. Twelve such files were
removed in the 2026-07 audit (no-op separator tweaks, an unreachable search page,
archetypes for content types that do not exist).
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

- **config.yml**: Site metadata, navigation menu, homepage bio (`params.profileMode.subtitle`), SEO description and keywords, social links, `params.schema` (JSON-LD identity).
- **Theme**: PaperMod, **vendored** — `themes/PaperMod/` is committed to this repo (regular files, no `.gitmodules`), not a git submodule. `git submodule update` does nothing here.
- **`taxonomies: {}`** disables tags and categories. Removing the key does NOT disable them: Hugo falls back to its defaults and generates empty `/tags/` and `/categories/` pages.
- **Custom domain**: none in effect — see Project Overview.

## Deployment

Automatic via GitHub Actions on push to `main`. Workflow at `.github/workflows/hugo.yml`. There is no `gh-pages` branch: the workflow builds into `public/` (gitignored locally), uploads it with `actions/upload-pages-artifact`, and `actions/deploy-pages` publishes the artifact.

## Accessibility & SEO

- Skip-to-content link, WCAG-compliant focus indicators, reduced-motion and high-contrast support (`assets/css/extended/accessibility.css`).
- Sitemap, canonical URLs, Open Graph, Twitter cards (theme defaults).
- Note: there is currently NO Google Scholar / Highwire Press citation metadata, because all papers live on a single listing page rather than per-paper pages. If Scholar indexing is desired in the future, restructure papers into `content/papers/<slug>/index.md` and re-add a `scholar_meta.html` partial gated on `eq .Section "papers"`.
