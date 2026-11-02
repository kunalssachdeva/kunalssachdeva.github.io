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

data/
└── agenda.yml          # Homepage research agenda: statement, band lead-in, four strands (see "Homepage agenda")

assets/css/extended/    # the ONLY stylesheets loaded (head.html no longer bundles the theme's CSS)
├── michigan-theme.css  # Tokens (all colours/fonts/sizes), @font-face, base, header, footer, homepage, chips
├── research-page.css   # Inner pages: page head, h2 sections, entries, Cite toggle, paper pages
└── accessibility.css   # Skip link, focus rings, reduced motion

layouts/                # every file here overrides or adds to themes/PaperMod/<same path>
├── index.html                # Homepage: bio block, statement, navy band, one section per strand
├── papers/single.html        # Paper pages: title, finding as lead, figure beside the record
├── 404.html
├── partials/
│   ├── head.html             # SEO meta, og:image / twitter:image, the stylesheet bundle
│   ├── header.html           # Skip link, Ross logo (home link), nav; maize on the current item
│   ├── footer.html           # Logo, school, email, section links, copyright; Cite copy button
│   ├── page.html             # Inner pages: title, intro, then each h2 + its entries as a section
│   ├── paper_entry.html      # One homepage answer, read from the paper's own page
│   ├── fn/typeset.html       # Non-breaking first/last words, hyphenated words kept whole
│   ├── fn/initials.html      # Keeps "Stephen M. Ross", "S. Lakshmi" together
│   └── scholar_meta.html, analytics.html, statcounter.html, templates/schema_json.html
└── _default/
    ├── baseof.html           # Base template; carries the #main-content skip target
    ├── single.html, list.html  # Both render partials/page.html
    └── _markup/render-link.html, render-heading.html

An override that no longer differs meaningfully from the theme is dead weight and
blocks theme updates — delete it rather than carrying it. Twelve such files were
removed in the 2026-07 audit (no-op separator tweaks, an unreachable search page,
archetypes for content types that do not exist).
```

The homepage bio is rendered from `params.profileMode.subtitle` in `config.yml`, not from a content file: paragraph 1 large under the name, paragraphs 3 and 4 stacked below it. Paragraph 2 (the research statement) is not shown there; the statement and the band come from `data/agenda.yml`, so keep the two in step when the agenda changes.

### Homepage agenda (`data/agenda.yml`)

The homepage is the research agenda: a statement (`statement.plain` + `statement.bold`, the bold run heavier), the navy band ("`lead`" followed by each strand's `clause`, joined into one sentence: commas, "and" before the last, a full stop), then one section per strand with its `question` pinned on the left.

Strands and papers render in the order listed. The environmental work leads (the owner's call, 2026-11): the most environmental strands come first ("Does pressure on firms really bite?", then "Are reported numbers real?"), and within each strand environmental papers come first, then the rest, published before working papers. To put a paper in a strand, add its slug under that strand's `papers` in that order:

```yaml
      - slug: real-effects-environmental-activist-investing
        number: "−13%"      # optional: only where the paper's finding states the figure
        unit: "yrs"         # optional: set smaller after the number ("4.8 yrs")
```

Everything else is read from `content/papers/<slug>/index.md`: the title, the finding (front-matter `description`), the authors (first body paragraph), the venue (second body paragraph) and the link chips (the `<a>`s in `<div class="paper-links">`, before `<details>`). Keep that body order when adding a paper page. A slug with no paper page, or a page missing those parts, fails the build with an `errorf` naming the slug. A strand's `id` is the band link's anchor (`#pressure`, `#numbers`, `#goals`, `#households`).


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

## Visual system ("Promise vs. reality", 2026-11)

A full redesign (stamp at the top of `michigan-theme.css`), ported from the owner's chosen mockup "d1a-refined". **Keep the white background**: the owner rejected an all-serif, tinted-paper version and a list-first homepage.

- **One 12-column grid**, capped at 1344px and centred (`.wrap` + `.grid`), left edges aligned from header to footer. The homepage questions and every inner page's h2 sections share one split: the head in columns 1-6, pinned (`position: sticky`) while the entries in columns 7-12 scroll past. Below 900px everything stacks.
- **One family**: Schibsted Grotesk, self-hosted in `static/fonts/` (variable 400-800, roman and italic, latin and latin-ext; OFL licence alongside). Weights 400, 700, 800 only. The type scale is the `--size-*` tokens (name 68, question 56, page title 52, statement 40, band 24, bio 23, finding 21, body 17, meta 16 at 1440).
- **Tokens only**: every colour, font and size is a token in the `:root` block of `michigan-theme.css`. No hex literal or `font-family` in a rule.
- **Navy** `#00274C`: the name, page titles, paper titles, chips, and the one full-bleed band (the agenda). **Maize** `#FFCB05` is a fill only, always with navy text: the headline numbers on the homepage and the current nav item. Never a line, never text on white.
- **Actions** are chips: sentence-case navy on `--color-chip`, solid navy on hover (Paper, PDF, Data, Code, NBER, ECGI, Cite, Slides). Never boxed outlines, never uppercase, never letter-spaced.
- **Links in running text** carry a quiet underline at rest (ink at 22%, 1px, offset .15em) and turn navy with a full-strength underline on hover; colour alone would fail WCAG 1.4.1.
- **No orphans**: heads use `text-wrap: balance`, paragraphs `pretty`; questions bind their first and last two words, findings their last two, initials stay with names, and hyphenated title words never break at the hyphen (`fn/typeset.html`, `fn/initials.html`, `render-heading.html`).
- **No `·` separators** in content: venue lines read like citations (`*Venue*, Month D, YYYY`; `[Outlet](url), “Headline,” Date`); lists of awards use semicolons. Working-paper lines are plain (`Working paper, 2026`); only journal names are italic.
- **No motion** beyond link state changes; the 1440x900 homepage fold must not cut a line of type (check after changing anything above the band).

Re-lint after a visual change: `node ~/.claude/skills/hallmark/scripts/sloplint.mjs assets/css/extended --genre editorial --render --url http://localhost:1313/ --url http://localhost:1313/research/` (needs `puppeteer-core`, or point `HALLMARK_PUPPETEER` at an install; landing-page rules such as 80 words a screen do not apply here). Pass only `assets/css/extended` as a file target, never `layouts/`: given template files it renders them raw and drops `.hallmark/*.png` folders inside `layouts/`, which Hugo then fails to parse, and `hugo server` silently keeps serving the last good build.

## Accessibility & SEO

- Skip-to-content link, WCAG-compliant focus indicators, reduced-motion and high-contrast support (`assets/css/extended/accessibility.css`).
- Sitemap, canonical URLs, Open Graph, Twitter cards (theme defaults).
- Google Scholar / Highwire Press `citation_*` tags are emitted on `/papers/<slug>/` pages by `layouts/partials/scholar_meta.html` from each page's `citation:` front matter. `citation_pdf_url` is emitted when the page sets `citation.pdf` (a file under `static/papers/<slug>.pdf`); Scholar requires the PDF on the same site.
