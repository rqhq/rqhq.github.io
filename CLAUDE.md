# Project: Ryan Quinlan Data Science Portfolio

Personal portfolio site to showcase undergrad and grad-school data science projects. Built with Quarto, hosted on GitHub Pages, deployed via the local-render-and-push workflow (no Actions).

## User context

Ryan Quinlan — data scientist. B.S. from University of Pittsburgh (2025), currently in M.S. Data Science at Drexel. Email: rwquinlan@outlook.com. GitHub: [@rqhq](https://github.com/rqhq). LinkedIn: ryan-w-quinlan.

Collaboration style: writes his own project descriptions but asks for tightening passes. Prefers terse responses. Open to recommendations but flag tradeoffs explicitly.

## Tech stack

- Quarto website project (not blog)
- Theme: `cosmo` (light) + `darkly` (dark) — toggleable
- `output-dir: docs` so GitHub Pages can serve from `main:/docs`
- `freeze: true` (notebooks render once, never re-execute)
- `body-width: 850px` for readability

## Repo layout

```
_quarto.yml          site config
index.qmd            homepage
about.qmd            about page
styles.css           minor custom CSS
projects/
  index.qmd          listing page (grid, auto-discovery, categories filter)
  _template.qmd      starting template (_ prefix = not rendered)
  Song_popularity.qmd
  Housing_Affordability.qmd
  Bucks_Hackathon.qmd
  Rail_Weather_Risk.qmd
pdfs/                static PDF assets (reports, slides)
notebooks/           Jupyter notebooks (currently empty)
images/              project thumbnails
docs/                rendered output (committed, served by Pages)
```

## Project page convention

Every project page follows roughly this structure:

```yaml
---
title: "..."
description: "..."         # one line, ~120 chars; appears on card
date: "YYYY-MM-DD"          # drives sort order on listing
categories: [tag1, tag2]    # populates filter UI on listing
image: ../images/foo.png    # card thumbnail (page renders fine if missing)
image-alt: "..."
format:
  html:
    code-fold: true
    code-tools: true
---

## Overview        — problem + approach + role/credit (1-2 paras)
## Tech stack      — slash-separated list of libraries
## Report          — <iframe> + download link (for PDF-centric projects)
## Presentation    — second <iframe> if there's a slide deck
## Key findings    — bullets with bolded leads
```

Code-first projects (e.g. Rail Weather Risk) skip the iframe and lead with a GitHub repo link instead.

## Quarto footguns we hit

1. **HTML comments do NOT disable Quarto shortcodes.** Wrapping `{{< embed file.ipynb >}}` in `<!-- -->` still triggers the shortcode and fails the build if the file is missing. Either delete the line, or use the literal-shortcode escape `{{</* ... */>}}`.
2. **PDF iframe via `<iframe src="../pdfs/foo.pdf">`** — works fine, but if the file is missing the iframe just shows a broken icon (does NOT fail the build). Different from the shortcode behavior above.
3. **`image:` in frontmatter degrades gracefully** if the file is missing (card renders without thumbnail). Inline `![](path)` does NOT — it shows a broken image icon. So for hero images, only add `![]()` after the file actually exists.
4. **Filenames with spaces in iframes work** in most browsers (auto URL-encoded), but kebab-case is more portable. Convention: kebab-case PDFs, snake_case .qmd page names.

## Deployment workflow (not yet executed)

1. `quarto render` → populates `docs/`
2. `git add docs/ && git commit && git push`
3. In repo settings, GitHub Pages → Source: `main` branch, `/docs` folder
4. Site live at `https://rqhq.github.io/`

This was step 5 of the original 6-step plan. As of last session, the user had not yet deployed.

## Current state

Five project pages scaffolded:

| Page | Status | Notes |
|------|--------|-------|
| Song_popularity.qmd | content done | Stat 1361 final, Pitt 2024-04-02. PDF embedded. |
| Housing_Affordability.qmd | content done | Capstone with Colton Dumm, 2025-04. Paper + slides both embedded. |
| Bucks_Hackathon.qmd | content done | 3rd place, 2024-02-13. Report + presentation both embedded. |
| Rail_Weather_Risk.qmd | content done | Drexel DSCI 611 group project, 2026-05. Code-first; GitHub link primary, test_model_eval.pdf embedded as evaluation artifact. |
| _template.qmd | template | Copy this for new projects. |

## Known TODOs / pending

- **Deploy to GitHub Pages** — never executed. Step 5 of the original plan.
- **`pdfs/test_model_eval.pdf` not yet copied in** — the Rail Weather Risk page references it but file isn't in `pdfs/` yet (user needs to copy from their `611-rail-weather-risk` repo).
- **README.md results table inconsistency** in [`rqhq/611-rail-weather-risk`](https://github.com/rqhq/611-rail-weather-risk) — README's tuned Negative Binomial MPD reads `11.07` but `test_model_eval.pdf` shows validation MPD is `10.07`. Worth reconciling.
- **Housing paper text vs Figure N** — paper says Perry County 2.07 and Delaware 3.21 in 2028, but Figure N shows Perry 2.98 and Delaware 3.40. Portfolio page uses the paper-text numbers; user should pick a source of truth.
- **DSCI 611 course title verification** — I labeled it "Data Acquisition and Pre-processing" on the Rail Weather Risk page based on a guess; the README only said "DSCI 611". Worth user-checking.

## Image / thumbnail status

All thumbnails currently exist:
- `bucks.png`
- `housing-affordability-thumb.png`
- `song-popularity-thumb.png`
- `train-thumb.png` (note: hyphen not underscore)
