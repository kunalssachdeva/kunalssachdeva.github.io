# HANDOFF — kunalssachdeva.github.io (written 2026-07-28)

Location: repo root of `kunalssachdeva/kunalssachdeva.github.io`. This repo is **public** — nothing sensitive belongs in this file.

Resume with: /handoff resume

---

## 1. Task

Improve the academic website at https://kunalssachdeva.github.io/. Started as a multi-agent audit, became a continuous improvement pass: correctness of the publication record, accessibility, SEO, scholarly-identity signals, and content gaps benchmarked against 14 peer academic sites. "Done" is open-ended; the work is tracked as a ranked roadmap (link in §4).

---

## 2. State

### Git
- Branch `main`, HEAD `18d6541`, working tree clean.
- **31 PRs merged** this session (#1–#31), each squash-merged and verified live after deploy.

### DONE (verified by fetching the deployed site, not just the build)

| Area | Result |
|---|---|
| Content correctness | 2 "Paper" links pointed at **other authors' papers** (SSRN 2846892, 3305647); 2 DOIs did not exist; 1 author order wrong. All fixed against Crossref. |
| SSRN links | 4 of 7 were wrong (3 pointed at unrelated papers, 1 at a removed record). Found via codex `gpt-5.6-terra`, re-verified against OpenAlex + Crossref. PR #6. |
| Slide decks | `2021-10-hedge-fund-activism.pdf` and `2021-10-mutual-funds-voting.pdf` were **Google sign-in HTML pages** served as PDFs. Replaced with originals from Google Drive. |
| Paper illustrations | All 11 SVGs regenerated on one grid; fixed clipping, overshoot, malformed arrowheads. PR #5. |
| Accessibility | h1 added, empty h1 fixed, skip-link focus, colour-only links, alt text. PR #1. |
| Structured data | Replaced PaperMod's 128-line `schema_json.html` with ~40. Fixed favicon-as-photo, truncated description, invalid `0001-01-01` dates. 9 verified `sameAs`. PR #22. |
| Pages added | `/media/` (30 items, dated, grouped by paper), `/teaching/`. |
| Working papers | All 6 dated from their own SSRN records via OpenAlex. 4 BibTeX years were wrong. PR #29. |
| BibTeX | 6 `@techreport` entries lacked required `institution`; acronyms unprotected; RFS entry claimed a year it did not have. PR #30. |
| Homepage | Bio rewritten several times; final = affiliation-first, 158 words, measure capped 34rem, justified, no hyphenation. |
| Cleanup | Removed inert `static/CNAME`, RSS (invalid dates), 3 `page/1/` stub URLs, theme footer credit, dead theme overrides, mis-attributed `LICENSE.md`. |

### IN FLIGHT
None. No background jobs running.

### NOT STARTED

**Blocked on owner (external platforms):**
1. **DNS / domain move** — highest impact, see §3.
2. **OpenAlex** author `A5077915577`: claim at openalex.org, remove 8 namesake works (`W2203538778`, `W2209617901`, `W2145459773`, `W36517893`, `W2251206152`, `W2251924162`, `W2251534017`, `W2807268528`), merge 3 author IDs (`A5140864244`, `A5134700376`, `A5127940386`).
3. **ORCID** `0000-0002-7739-0073` — only 4 works, no education history.
4. **RePEc** `psa1819` — only 3 items.
5. **Google Search Console** — not verified; nothing else reveals real query data.
6. **Self-hosted working-paper PDFs** — blocked: the files are not available to the assistant and SSRN blocks automated fetching.

**Available to do on the site:**
7. Per-paper landing pages (5 indexable URLs → 16; biggest remaining SEO change).
8. Missing discussion: *Unsafe Waters*, Lorena Moreno, NEUDC — on the CV, absent from `/discussions/`.
9. From the codex second opinion: research-agenda statement, referee-fit/expertise strip, plain-text email, two over-claiming abstracts.
10. **Analytics — requested 2026-07-28, in progress at the time of writing.** Owner currently uses StatCounter and values seeing visitor IP + location.

---

## 3. Decisions

Made and ratified this session:
- **Stay on `kunalssachdeva.github.io` for now**; DNS deferred by owner.
- **The CV is authoritative** where CV and site disagreed → RFS "Accepted", Skin or Skim 2024, email `ksach@umich.edu`.
- **No separate bio page** — the long bio lives on the homepage instead (a `/bio/` page was built, then removed).
- **Affiliation leads the homepage bio**, reversing an earlier research-first ordering.
- **No hyphenation** in the justified bio.
- **Doctoral advising removed** from `/teaching/` (0 of 14 peer sites list it).
- **RSS disabled** rather than repaired.
- **LinkedIn excluded** from `sameAs` — returns 999 to automated requests, so it could not be verified.
- **Biodiversity from Space** working paper deliberately NOT added to the site.

### Open / deferred — must re-surface on resume
- **The R&R question.** The owner asked to remove "R&R, *Review of Financial Studies*" from *Can Health State-Contingent Assets…*. A later codex review, wearing a tenure-committee hat, called its absence "a material self-inflicted downgrade" because it is the clearest solo-authored top-journal signal in the record. Not re-added. Owner's call.
- **`year={2026}`** on the forthcoming RFS BibTeX entry is a forward-looking placeholder; correct it when a volume is assigned.
- **`static/cv.pdf` page 1 carries personal contact details and citizenship status.** It is a crawlable PDF on a public domain. Decide whether to publish a web-safe version.
- **Financial Times media item** has no recoverable date or headline (403 to all automated access).
- **60% vs 74% emissions figures** — reconciled with a note on `/media/`; no firm-level figure was added to `/research/` because Crossref carries no abstract to source it from.

---

## 4. Next steps

1. **Analytics** (active request). Recommend a cookieless option; owner wants visitor IP/location, which the privacy-respecting tools deliberately do not provide — resolve that tension first.
2. **DNS move.** Set `www` CNAME → `kunalssachdeva.github.io`, apex → GitHub's four A records; then add `static/CNAME` and set `baseURL` in `config.yml`; then add Hugo `aliases` for the old Google Sites paths `/home`, `/research`, `/code-and-data`, `/discussion-slides`; then retire the Google Site; then update the homepage URL on the Google Scholar profile and ask Ross web to update the faculty page link.
3. **OpenAlex / ORCID / RePEc** cleanup per §2.
4. Add the missing *Unsafe Waters* discussion to `content/discussions.md`.
5. Per-paper landing pages under `content/papers/<slug>/`.

**Roadmap artifact** (private, fuller detail, includes the 14-site peer feature matrix):
https://claude.ai/code/artifact/37558bea-d4e5-417b-8b3b-0778a36008bb

---

## 5. Gotchas

- **The working clone was ephemeral**: `/Users/ksach/.claude/jobs/c527ff99/tmp/site` is a job temp directory and will be deleted. Re-clone from GitHub. Anything not committed is gone — including `gen_svgs.py`, the generator that produced the 11 paper SVGs.
- **`hugo` is not on PATH by default.** Use `/opt/homebrew/bin/hugo` (0.164.0). CI pins `HUGO_VERSION: 0.164.0`.
- **`taxonomies: {}` in `config.yml` is load-bearing.** Deleting the key does *not* disable taxonomies — Hugo falls back to its defaults and generates empty `/tags/` **and** `/categories/` pages.
- **The theme's `render-image` hook drops empty attributes**, so `![](...)` yields an `<img>` with *no* alt attribute rather than `alt=""`. Use descriptive alt text.
- **`layouts/` files override `themes/PaperMod/<same path>`.** Deleting an override silently falls back to the theme version — verify build output after any deletion.
- **`grep -c` on minified single-line files counts *lines*, not occurrences.** This produced a false "sitemap regression" alarm. Use `grep -o … | wc -l`.
- **Blocks automated fetching** (403/401/999): SSRN, LinkedIn, Financial Times, Bloomberg, Reuters, MoneyWeek, ValueWalk, MarketWatch, the OpenAlex help centre, and the **Altmetric API** (403s every DOI, including controls — read the score off the Nature article page instead).
- **PaperMod's `min-height: 100vh` on `.profile`** pinned the homepage to one screen; overridden in `michigan-theme.css`.
- **A second Claude session edited this repo concurrently** on 2026-07-27 (PRs #3 and #4). Check `git log` before assuming your local state is current.
- CI runs on push to `main` only — nothing validates a PR before merge.
