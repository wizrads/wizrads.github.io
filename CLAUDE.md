# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A personal academic website built from the [Academic Pages](https://github.com/academicpages/academicpages.github.io)
Jekyll template (itself a fork of Minimal Mistakes). The remote is `wizrads/wizrads.github.io`, GitHub Pages builds
and serves it, and `www.joeyschulz.com` / `joeyschulz.com` are custom domains pointing at that Pages site (apex A
records to GitHub's IPs, `www` CNAME to `wizrads.github.io`, which redirects to the apex). The custom domain is set
both in the repository's Pages settings and in the root `CNAME` file — keep the two in sync; deleting `CNAME` can
drop the domain on the next deploy.

Pages is configured as "Deploy from a branch": **`master`, `/ (root)`**, built by GitHub's own `pages build and
deployment` workflow. `master` is the default branch, so pushing to it publishes.

**Workflow: commit and push straight to `master`.** No feature branches, no pull requests — this is a single-author
personal site and `master` is the deploy branch, so a PR only delays the change going live. Verify the build first
(`bundle exec jekyll build --strict_front_matter`), then commit and `git push origin master`; the Pages deploy takes
roughly a minute, after which the change can be confirmed against https://joeyschulz.com.

Site-wide identity (`_config.yml` title/description and the whole `author:` sidebar block) is filled in. The
*content* is still unmodified upstream template material — the placeholder files in `_posts`, `_talks`,
`_publications`, `_teaching`, and `_portfolio`, the homepage body in `_pages/about.md`, and the
`_pages/markdown.md` / `terms.md` docs pages. When personalizing, edit or delete those files rather than adding
parallel ones.

Source material for the real content lives outside the repo: Joey's CV (publications with DOIs, grants, awards,
teaching) and his Google Scholar profile. `markdown_generator/` exists to turn that kind of tabular/BibTeX data into
`_publications/` and `_talks/` entries.

## Commands

```bash
bundle install                              # Ruby deps (delete Gemfile.lock first if it errors; the lock is gitignored)
bundle exec jekyll serve -l -H localhost    # dev server with livereload at localhost:4000
bundle exec jekyll build --strict_front_matter   # what CI runs; use this to check a change builds
docker compose up                           # containerized alternative (_config.yml + _config_docker.yml, port 4000)
```

Editing `_config.yml` requires restarting Jekyll; Markdown/HTML changes hot-reload.

JavaScript is committed pre-built. After touching anything under `assets/js/` (except `main.min.js`):

```bash
npm install && npm run build:js             # uglifies jquery + greedy-nav + _main.js + theme.js into assets/js/main.min.js
```

Commit the regenerated `assets/js/main.min.js` — the site loads only that file.

There is no test suite. CI is `.github/workflows/jekyll-build.yml` (a strict-front-matter build); note it triggers on
branch `main`, not this repo's `master`, so it does not actually gate pushes here.

## Architecture

**Content is data, layout is theme.** Each content type is a Jekyll collection of Markdown files whose YAML front
matter drives multiple rendered surfaces:

- `_publications/`, `_talks/`, `_teaching/`, `_portfolio/` — collections declared in `_config.yml` with
  `permalink: /:collection/:path/`. Filenames are date-prefixed (`YYYY-MM-DD-slug.md`).
- `_posts/` — blog posts, permalink `/:categories/:title/`.
- `_pages/` — standalone pages; each declares its own `permalink`. The listing pages (`publications.html`,
  `talks.html`, `teaching.html`, `portfolio.html`, `year-archive.html`) iterate the collections above.
- One talk file feeds the talks list, its own page, the CV talks section, and `talkmap.html`. Changing front-matter
  keys ripples across all of them.

**Rendering chain:** `_layouts/` (`default`, `single`, `archive`, `archive-taxonomy`, `talk`, `cv-layout`, `splash`,
plus `compress.html` which minifies output) compose `_includes/` partials — `head/`, `masthead.html`,
`sidebar.html`, `author-profile.html`, `archive-single*.html`, `seo.html`, `footer/`, `scripts.html`, `toc/`.

**Site-wide data:**
- `_config.yml` — identity (`title`, `name`, `description`, `url`, `repository`), the author/sidebar block, analytics
  and comments provider selection, and `site_theme`. `url` is `https://joeyschulz.com` and must stay equal to the
  custom domain (not `wizrads.github.io`) — canonical tags, Open Graph URLs, the feed, and every absolute asset link
  are built from it.
- `_data/navigation.yml` — top menu. `_data/authors.yml` — multi-author bylines. `_data/ui-text.yml` — UI strings/i18n.
- `_data/cv.json` — JSON Resume data consumed by `_includes/cv-template.html` for the `/cv-json/` page.

**Theming:** `site_theme` in `_config.yml` selects one of `default`, `air`, `sunrise`, `mint`, `dirt`, `contrast`;
each has a light and dark variant in `_sass/theme/`, wired up by `_sass/_themes.scss`. The masthead toggle switches
light/dark at runtime (`assets/js/theme.js`). Prefer changing theme variables over hardcoding colors in layouts.

**Two parallel CVs.** `_pages/cv.md` is a hand-written Markdown CV at `/cv/`; `_pages/cv-json.md` at `/cv-json/`
renders `_data/cv.json`. `scripts/update_cv_json.sh` (wrapping `scripts/cv_markdown_to_json.py`) regenerates the JSON
from the Markdown, so treat `cv.md` as the source of truth and re-run that script instead of hand-editing `cv.json`.

**Generators (optional, run manually):**
- `markdown_generator/` — Jupyter notebooks and equivalent `.py` scripts turning TSV/CSV/BibTeX/ORCID data into
  `_publications/` and `_talks/` Markdown files.
- `talkmap.py` / `talkmap.ipynb` — geocodes `_talks/` locations into `talkmap/` for the `/talkmap.html` page. The
  `scrape_talks.yml` workflow runs this on pushes touching `_talks/` and commits the result back.

`_site/` and `.sass-cache/` are build output and gitignored.

## Upstream template quirks

`.github/workflows/bad-pr.yml` and `close-tests.yml` exist to police the *upstream template's* issues and PRs
(auto-closing PRs with empty bodies and issues mentioning "test"). They are irrelevant to a personal fork and are
safe to delete. `README.md`, `_pages/markdown.md`, and `_pages/terms.md` are likewise upstream template docs.
