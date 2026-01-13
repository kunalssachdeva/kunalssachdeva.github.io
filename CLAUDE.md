# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Academic website for Kunal Sachdeva, Assistant Professor of Finance at University of Michigan Ross School of Business. Built with Hugo static site generator using the PaperMod theme, hosted on GitHub Pages.

## Build Commands

```bash
# Install dependencies (first time only)
brew install hugo

# Local development server with live reload
hugo server

# Production build
hugo --gc --minify

# Build with draft content
hugo server -D
```

## Project Structure

```
content/
├── papers/          # Research papers (each paper has its own folder with index.md)
├── data/            # Code & Data section
├── discussions/     # Discussion slides
├── media/           # Media/Press mentions
└── archive.md       # Archive page

layouts/
├── partials/
│   ├── scholar_meta.html    # Google Scholar meta tags for papers
│   └── head.html            # Modified to include SEO features
└── _default/
    └── baseof.html          # Base template with accessibility features

assets/css/extended/
└── accessibility.css        # Custom accessibility styles

static/
├── CNAME            # Custom domain configuration
└── cv.pdf           # CV file (to be added)
```

## Adding New Content

### New Research Paper
Create folder in `content/papers/paper-slug/index.md`:

```yaml
---
title: "Paper Title"
date: 2024-01-01
tags: ["Tag1", "Tag2", "Published"]  # or "Working Paper", "R&R"
author: ["Author One", "Author Two"]
description: "Brief description for SEO and meta tags"
summary: "One-line summary for listings"
editPost:
    URL: "https://journal-url.com"
    Text: "Journal Name"
---
```

### Adding Media Mentions
Edit `content/media/_index.md` and add entries in markdown format.

## Key Configuration

- **config.yml**: Site metadata, navigation, social links, SEO keywords
- **Theme**: PaperMod (via git submodule in `themes/PaperMod/`)
- **Custom domain**: www.kunalsachdeva.com (configured in `static/CNAME`)

## Deployment

Automatic deployment via GitHub Actions on push to `main` branch. Workflow defined in `.github/workflows/hugo.yml`.

## SEO Features

- Google Scholar meta tags (`layouts/partials/scholar_meta.html`) for academic paper pages
- Sitemap generation enabled
- Canonical URLs
- Open Graph and Twitter cards (via theme)

## Accessibility

- Skip-to-content link
- WCAG-compliant focus indicators
- Reduced motion support
- High contrast mode support
- Semantic HTML structure
