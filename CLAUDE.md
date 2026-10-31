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
├── cv.pdf              # Linked from nav; GENERATED from the CV repo, see "CV" below
├── profile.jpg         # Homepage portrait
├── files/slides/       # Discussion slide PDFs (naming convention: YYYY-MM-keyword.pdf)
└── images/papers/      # Per-paper SVG illustrations referenced from research.md

assets/css/extended/
├── michigan-theme.css  # Tokens (all colours/fonts), @font-face, header, homepage
├── research-page.css   # Paper/discussion/media entries and paper pages
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

### Paper illustrations (`static/images/papers/*.svg`)

One visual system, and the rules are load-bearing:

- **Geometry must encode the number it prints.** If a figure says "+12%", the taller bar is exactly 1.12x the shorter one. The original patent-gender.svg drew a 31% gap while labelling it 12%.
- **`viewBox="0 0 320 200"`** — matches the CSS display width, so `font-size: 11` renders as 11px. Never go below 11.
- **Palette**: `#00274C` navy (data), `#7A93AC` tint (comparison/reference; 3.18:1 on white, so the surface must stay white), `#4a4a4a` labels, `#666` caption, `#d8dde3` rules.
- **Maize `#FFCB05` is a fill, never a stroke.** It is 1.52:1 on white — invisible as a line — but navy on maize is 9.89:1, so it works as a highlight block with navy text on it.
- **No background rect.** `.post-content img` in research-page.css owns the surface and border.
- Alt text states the *finding*, not the paper title (the title is already in the adjacent `h3`).

The research page has two sections: "Published & Accepted Papers" (sorted with newest acceptance first) and "Working Papers" (R&Rs first, then plain working papers).

### New discussion
Edit `content/discussions.md`. Entries are grouped by year (newest first), with the venue and date formatted as `*Conference Name*, Month D, YYYY`. Author links point to faculty pages for professors; PhD students/job-market candidates are listed without a link. Save slides under `static/files/slides/YYYY-MM-keyword.pdf` to match the existing naming convention.

### Code & Data
Edit `content/data/_index.md` to add a project repo or replication package.

### CV
The CV source is `cv.tex` in the private repo `kunalssachdeva/Kunal-Sachdeva-CV`. Its
`publish-cv.yml` workflow compiles it on every push to that repo's `main` and commits the
PDF here as `static/cv.pdf` (author `github-actions[bot]`, via a write deploy key), which
triggers the Hugo deploy. Never edit `static/cv.pdf` by hand: change `cv.tex` instead, or
the next CV push overwrites your change.

`reference/cv.tex` is a local, gitignored symlink to `../Kunal-Sachdeva-CV/cv.tex`, a clone
sitting next to this repo; read it for current CV facts (titles, dates, awards) when editing
site content. It is only as fresh as that clone: `git -C ../Kunal-Sachdeva-CV pull` first.
Never commit the .tex here — this repo is public, the CV repo is not, and the source carries
commented-out material that the PDF does not.

## Key Configuration

- **config.yml**: Site metadata, navigation menu, homepage bio (`params.profileMode.subtitle`), SEO description and keywords, social links, `params.schema` (JSON-LD identity).
- **Theme**: PaperMod, **vendored** — `themes/PaperMod/` is committed to this repo (regular files, no `.gitmodules`), not a git submodule. `git submodule update` does nothing here.
- **`taxonomies: {}`** disables tags and categories. Removing the key does NOT disable them: Hugo falls back to its defaults and generates empty `/tags/` and `/categories/` pages.
- **Custom domain**: none in effect — see Project Overview.

## Deployment

Automatic via GitHub Actions on push to `main`. Workflow at `.github/workflows/hugo.yml`. There is no `gh-pages` branch: the workflow builds into `public/` (gitignored locally), uploads it with `actions/upload-pages-artifact`, and `actions/deploy-pages` publishes the artifact.

## Visual system ("Michigan modern", 2026-10)

Restyled against the Hallmark anti-slop gates (`~/.claude/skills/hallmark`; stamp and waivers at the top of `michigan-theme.css`). The owner chose this from three previews and rejected an all-serif, tinted-paper version: **keep the white background**.

- **One family**: Schibsted Grotesk, self-hosted in `static/fonts/` (variable 400-800, roman and italic, latin and latin-ext; OFL licence alongside). Weights in use: 400, 700, 800 only.
- **Tokens only**: every colour and font is a `--color-*` / `--font-*` token in the `:root` block of `michigan-theme.css`. No hex literal or `font-family` in a rule.
- **Navy** `#00274C`: the name, page titles, links, the Research button, chip text. **Maize** `#FFCB05` is a fill only (section heads such as "Published & Accepted Papers" and years, the current nav item), always with navy text.
- **Actions**: paper links and Slides are sentence-case navy chips on `--color-chip`; homepage Research is the one solid navy button, CV the quiet chip. Never boxed outlines, never uppercase, never letter-spaced.
- **No `·` separators** in content: venue lines read like citations (`*Venue*, Month D, YYYY`; `[Outlet](url), “Headline,” Date`); lists of awards use semicolons. Working-paper lines are plain (`Working paper, 2026`), not italic: only journal names are italic.
- **Links** carry an underline at rest (navy on body ink is only 1.46:1, so colour alone fails WCAG 1.4.1). Paper titles too: they are the only link to `/papers/<slug>/`.

Re-lint after a visual change: `node ~/.claude/skills/hallmark/scripts/sloplint.mjs assets/css/extended --genre editorial --render --url http://localhost:1313/ --url http://localhost:1313/research/` (needs `puppeteer-core`; landing-page rules such as 80 words a screen do not apply here). Pass only `assets/css/extended` as a file target, never `layouts/`: given template files it renders them raw and drops `.hallmark/*.png` folders inside `layouts/`, which Hugo then fails to parse, and `hugo server` silently keeps serving the last good build.

## Accessibility & SEO

- Skip-to-content link, WCAG-compliant focus indicators, reduced-motion and high-contrast support (`assets/css/extended/accessibility.css`).
- Sitemap, canonical URLs, Open Graph, Twitter cards (theme defaults).
- Google Scholar / Highwire Press `citation_*` tags are emitted on `/papers/<slug>/` pages by `layouts/partials/scholar_meta.html` from each page's `citation:` front matter. `citation_pdf_url` is emitted when the page sets `citation.pdf` (a file under `static/papers/<slug>.pdf`); Scholar requires the PDF on the same site.
