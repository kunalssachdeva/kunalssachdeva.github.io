# kunalssachdeva.github.io

Academic website for [Kunal Sachdeva](https://kunalssachdeva.github.io/), Assistant
Professor of Finance at the University of Michigan Ross School of Business.

Hugo + a vendored copy of the [PaperMod](https://github.com/adityatelange/hugo-PaperMod)
theme, deployed to GitHub Pages by `.github/workflows/hugo.yml` on every push to `main`.

## Local development

```bash
brew install hugo     # first time only
hugo server           # live reload at http://localhost:1313
hugo --gc --minify    # production build into public/
```

## Editing content

All content is flat Markdown — one file per page, not one folder per item.

| Page | File |
| --- | --- |
| `/research/` | `content/research.md` |
| `/discussions/` | `content/discussions.md` |
| `/data/` | `content/data/_index.md` |
| homepage bio | `params.profileMode.subtitle` in `config.yml` |

Slides go in `static/files/slides/` as `YYYY-MM-keyword.pdf`; paper illustrations in
`static/images/papers/`. `CLAUDE.md` has the per-entry formatting conventions.

## Theme

`themes/PaperMod/` is committed to this repo, not a git submodule. Files under
`layouts/` and `assets/css/` override the theme copy of the same path; keep an
override only when it differs meaningfully, since each one blocks a theme update.
