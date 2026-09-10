# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

Joey Schulz's personal academic website, built from the
[Academic Pages](https://github.com/academicpages/academicpages.github.io) Jekyll template (itself a fork of Minimal
Mistakes). The remote is `wizrads/wizrads.github.io`; GitHub Pages builds and serves it at **https://joeyschulz.com**
(apex A records to GitHub's IPs, `www` CNAME to `wizrads.github.io`, which 301s to the apex). Enforce HTTPS is on.

The custom domain is set both in the repository's Pages settings and in the root `CNAME` file — keep the two in sync;
deleting `CNAME` can drop the domain on the next deploy.

Pages is configured as "Deploy from a branch": **`master`, `/ (root)`**, built by GitHub's own `pages build and
deployment` workflow.

## Workflow

**Commit and push straight to `master`.** No feature branches, no pull requests — single-author personal site, and
`master` is the deploy branch, so a PR only delays the change going live.

1. `bundle exec jekyll build --strict_front_matter`
2. commit, `git push origin master`
3. the Pages build takes about a minute; then verify against https://joeyschulz.com

`gh` is installed at `/opt/homebrew/bin/gh` and authenticated as `wizrads`, but that directory is not always on
`PATH` — prefix with `export PATH="/opt/homebrew/bin:$PATH"`. To watch a deploy:
`gh api repos/wizrads/wizrads.github.io/pages/builds/latest --jq '{status,commit}'`.

## Commands

```bash
bundle install                                   # Ruby deps (delete Gemfile.lock first if it errors; the lock is gitignored)
bundle exec jekyll serve -l -H localhost         # dev server with livereload at localhost:4000
bundle exec jekyll build --strict_front_matter   # what CI would run; use this to check a change builds
npm install && npm run build:js                  # rebuild assets/js/main.min.js
docker compose up                                # containerized alternative (_config.yml + _config_docker.yml, port 4000)
```

Editing `_config.yml` requires restarting Jekyll; Markdown/HTML changes hot-reload.

**Always preview with `jekyll serve`, never by serving `_site/` with another static server.** `url` is
`https://joeyschulz.com`, so a plain `jekyll build` bakes absolute production URLs into every asset link — the page
then silently loads the *live* CSS and JS and your local changes appear to do nothing. `jekyll serve` rewrites `url`
to the local address for the duration.

**JavaScript is committed pre-built.** `main.min.js` is jquery + `plugins/jquery.greedy-navigation.js` + `_main.js` +
`theme.js` uglified together, and it is the only script the site loads. After editing any of those sources, run
`npm run build:js` and commit the regenerated bundle.

There is no test suite. CI is `.github/workflows/jekyll-build.yml`, but it triggers on branch `main` while this repo
uses `master`, so it never actually runs. Verify builds locally.

**A green local build does not guarantee a green Pages build.** GitHub Pages force-enables
`jekyll-optional-front-matter`, which turns *every* front-matter-less `.md` file in the repo into a page and runs it
through Liquid — local Jekyll does not, and copies those files verbatim. `CLAUDE.md` quotes `{{` and `{%`, so Pages
choked on it and the deploy failed while `jekyll build` stayed clean; it is now in `exclude:` in `_config.yml`. Any
new Markdown at the repo root needs front matter, an `exclude:` entry, or no Liquid braces. If a push does not show
up on the live site, check `gh run list --repo wizrads/wizrads.github.io`, not just the build status.

## Content

All template placeholder content is gone. `_config.yml`, `_pages/about.md`, `_pages/cv.md`, and the `_publications`
(17), `_talks` (28), `_teaching` (2), and `_portfolio` (4) collections hold real content generated from Joey's CV.
`_posts/` is empty and there is no blog. The nav is Publications / Talks / Teaching / Portfolio / Projects.

**Dates are year-accurate only.** The CV supplies publication and meeting years but not days, so every generated file
uses `YYYY-01-01`. Never present those days as real.

**Attribution matters to Joey and is not inferable from author order.** He is first author on the AVATAR 2.0 and
3D-printed device papers but asked that both be described as collaborations he contributed to rather than led; the
`_portfolio` entries and the homepage's Background paragraph say so explicitly. Ask before characterizing his role on
anything new.

Source material lives outside the repo: his CV (`~/Downloads/01-Schulz_CV_AI_update.docx` as of Sep 2026) and his
[Google Scholar profile](https://scholar.google.com/citations?user=EK1HF2oAAAAJ). When adding publications or talks
in bulk, copy the front-matter shape of an existing file rather than using the `markdown_generator/` notebooks, which
expect their own TSV/BibTeX inputs.

### Paper figures

`images/publications/` holds one figure per paper, embedded near the bottom of the publication's Markdown body and
also set as `header.teaser` in front matter. Only **3 of 17** are done (the three Frontiers papers, which are CC BY).
The rest are blocked on two things: the paywalled journals do not make figures retrievable, and *which* figure counts
as the main one is Joey's editorial call, not a guess to make. Ask him for the files.

### Projects (self-hosted demos)

Proof-of-concept apps are served from this same repo and domain. The layout is deliberate:

- `_pages/projects.md` owns the `/projects/` permalink and is the styled listing page, in the site's own layout.
- Each demo is a plain static folder, `projects/<slug>/index.html`, served at `/projects/<slug>/` with no site chrome.

**Never put an `index.html` at `projects/` root** — it and `_pages/projects.md` would both write
`_site/projects/index.html` and one silently wins. Add a demo by dropping in `projects/<new-slug>/` and adding a
section to the listing page.

Static files with **no YAML front matter are copied byte-for-byte** and never run through Liquid, which is what makes
dropping a prebuilt app in here safe. A file that *does* have front matter, or any `{{` / `{%` in a template or JS
library, will be mangled — check with `grep -c '{{\|{%'` before adding one.

House rule for these demos, applied to the first one: **no external requests.** Bundle libraries inline and strip
web-font links (the alumni app had three Google Fonts tags removed and its stack re-led with `system-ui`). Verify
with `performance.getEntriesByType('resource').filter(r => !r.name.startsWith(location.origin))` — it should be
empty. Remember GitHub Pages is static-only, and anything here is public, so no secrets or API keys ever.

Current demo: `projects/uw-madison-alumni-network/` — a D3 map of UW–Madison Medical Physics graduates 2019–2026.
Its roster is embedded in the page and its "Load roster CSV" control reads a local file in the browser only; the CSV
is **deliberately not shipped**. Its "First destination" panel reports categories only and does not publish
employers — keep it that way if the data is ever extended.

### Brainrot mode (the Gen Alpha toggle)

The homepage carries a joke switch that swaps its copy for a Gen Alpha translation. Four moving parts:

- `_data/brainrot.yml` — the alternate copy: `page:` (keyed by block), `nav:` (keyed by rendered nav label), `bio:`.
  Values are injected as HTML, so a translation must carry over any links the real copy had.
- `_pages/about.md` — each translatable block is tagged with a kramdown inline attribute list,
  `{: data-brainrot="intro-lab"}` on the line after it. `{:` is not Liquid, so it survives the build; the IAL does
  not disturb the heading ids `auto_ids` generates. The bullets are tagged as **one** list (`research-list`), so
  that translation supplies all four `<li>` elements.
- `_includes/brainrot-toggle.html` — the switch plus `<script type="application/json" id="brainrot-data">`, which is
  just `site.data.brainrot | jsonify`. Included at the top of `about.md`.
- `setupBrainrot()` in `assets/js/_main.js` (styles in `_sass/include/_brainrot.scss`) — reads that JSON, stores each
  element's real `innerHTML`, and swaps. It returns immediately when the include is absent, so every other page is
  untouched. The state persists in `localStorage.brainrot`.

A block whose text is rewritten but whose key is left in place still renders fine — it just stops translating, so
**editing the real copy can never break the page**. Keep the two in sync by hand. Nav labels are swapped in place,
which means the greedy nav has to re-measure: `renderBrainrot()` triggers a `resize` for that reason. Only the
homepage is translated, so the nav labels revert when you navigate away.

**Attribution still applies here.** The `background-stanford` translation keeps the "team projects I was part of"
framing for the 3D-printed devices and AVATAR 2.0 (see **Content** above) — do not let a punchier rewrite turn those
into things he led.

### Hidden pages

`/cv/` is deliberately hidden but not deleted: its nav entry is commented out in `_data/navigation.yml` and it carries
`sitemap: false`. Note that flag needs **two** cooperating pieces — `jekyll-sitemap` reads it for `sitemap.xml`, and
`_pages/sitemap.md` was patched to honour it as well, because that human-readable sitemap is linked from the footer of
every page and would otherwise keep the page one click away. Use the same pattern for anything else to be hidden.

`/cv-json/` (`_pages/cv-json.md` + `_data/cv.json`) is unlinked, still in `sitemap.xml`, and only partly populated:
`scripts/cv_markdown_to_json.py` demands a rigid `Degree, Institution, YYYY` bullet format that the real CV does not
fit, so its Education and Work sections come out empty. Either fix the script or delete the page — do not contort
`cv.md` to satisfy the parser.

Known gaps: no ORCID iD in the sidebar, no `files/cv.pdf` for the download link, real talk dates outstanding.

## Architecture

**Content is data, layout is theme.** Each content type is a Jekyll collection whose YAML front matter drives several
rendered surfaces at once:

- `_publications/`, `_talks/`, `_teaching/`, `_portfolio/` — collections declared in `_config.yml` with
  `permalink: /:collection/:path/`; filenames are date-prefixed (`YYYY-MM-DD-slug.md`).
- `_pages/` — standalone pages, each declaring its own `permalink`. The listing pages (`publications.html`,
  `talks.html`, `teaching.html`, `portfolio.html`) iterate the collections.
- One talk file feeds the talks list, its own page, the CV's Talks section, and `talkmap.html`. Changing a
  front-matter key ripples across all of them.
- `_pages/cv.md`'s Publications / Talks / Teaching sections are Liquid loops over the collections, so adding a file
  to `_publications` updates the CV automatically.

**Rendering chain:** `_layouts/` (`default`, `single`, `archive`, `archive-taxonomy`, `talk`, `cv-layout`, `splash`,
plus `compress.html`, which minifies output) compose `_includes/` partials — `head/`, `masthead.html`, `sidebar.html`,
`author-profile.html`, `archive-single*.html`, `seo.html`, `footer/`, `scripts.html`, `toc/`.

**Site-wide data:**
- `_config.yml` — identity, the `author:` sidebar block, `publication_category` (currently `manuscripts` and
  `preprints`), analytics/comments providers, and `site_theme`. `url` must stay equal to the custom domain, since
  canonical tags, Open Graph URLs, the feed, and every absolute asset link are built from it.
- `_data/navigation.yml` — top menu. `_data/authors.yml` — multi-author bylines. `_data/ui-text.yml` — UI strings.
- The sidebar avatar is `images/profile.jpg`, referenced by `author.avatar` as a bare filename.

**Theming:** `site_theme` is `default`; each theme has light and dark variants in `_sass/theme/`, wired up by
`_sass/_themes.scss`. The masthead toggle switches light/dark at runtime (`assets/js/theme.js`). Prefer changing theme
variables over hardcoding colors.

**Generators (manual):** `markdown_generator/` turns TSV/CSV/BibTeX/ORCID data into collection files.
`talkmap.py` / `talkmap.ipynb` geocode `_talks/` locations for `/talkmap.html`; the `scrape_talks.yml` workflow runs
the notebook on pushes touching `_talks/` and commits the result back.

`_site/` and `.sass-cache/` are build output and gitignored.

## Local changes to theme files — do not lose these in an upstream merge

Several template files were patched to fix visible bugs. They look like innocuous theme code and are easy to revert
by accident.

- **`_sass/theme/_default_{light,dark}.scss`, `_sass/layout/_base.scss`, `plugins/jquery.greedy-navigation.js`** —
  `$masthead-height` (65px) and `$masthead-height-narrow` (58px) must equal the masthead's *measured* height, because
  `body` reserves those values and the greedy-nav JS measures the real thing and corrects any mismatch after first
  paint, which shows up as the whole page jumping. The JS now only writes that padding when it disagrees by more than
  2px. **If you change the theme, the nav items, or the masthead's font/padding, re-measure and update both
  constants** (`document.querySelector('.masthead').getBoundingClientRect().height` at a wide and a narrow viewport).
  `$masthead-height-narrow` is declared with `!default` in `_sass/_themes.scss` so other themes still compile.
- **`_includes/masthead.html`** — the nav's overflow button ships with `class="hidden"` (plus an `aria-label` it
  never had). Upstream rendered it visible and let the JS hide it, which flashed a hamburger on every page load.
- **`_includes/author-profile.html`** — the avatar `<img>` must **not** carry `class="author__avatar"`; that class
  belongs to its wrapper `<div>`. Upstream put it on both, so the wrapper's `display: table-cell; width: 36px`
  landed on the image and collapsed it to 0×0 below the `$large` breakpoint — which reads as "the photo doesn't
  load". Note the other branch of that same `if/else`, used when `avatar` is a full URL, never had the bug.
- **`_sass/include/_utilities.scss`** — `.author__urls i` / `.social-icons i` reserve a `1.25em` box. Font Awesome
  and Academicons are loaded with `rel="preload"` and only become stylesheets after first paint, so without this the
  icons have no width and every sidebar row's text shifts ~10px sideways once they land.
- **`_sass/layout/_footer.scss` + `_includes/footer.html`** — the footer is one compact flex row (~46px, was ~93px).
  The `@include clearfix` was removed from that rule deliberately: its `::before`/`::after` pseudo-elements become
  flex items and force the copyright onto its own line.
Navigation is a plain full page load. Hover-prefetching (instant.page) was added and then **deliberately reverted**
at Joey's request — do not reintroduce it or a client-side router without asking.

The staggered `intro` fade-in (masthead 0.15s, `#main` 0.35s, footer 0.45s) is upstream behaviour and is **kept**.
It was briefly deleted on the theory that it was what made navigation feel like a reload; that was wrong. The fade is
what makes the upstream demo look smooth, and the computed animations here now match
https://academicpages.github.io exactly. What actually looked broken was three real bugs firing underneath it — the
masthead padding jump, the flashing nav button, and the shifting sidebar icons, all fixed above. Do not remove the
fade to "speed up" navigation.

## Upstream template leftovers

`.github/workflows/bad-pr.yml` and `close-tests.yml` police the *upstream template's* issues and PRs — the first
auto-closes any PR whose body is empty or contains "by deleting this comment block", which is a phrase in this repo's
own `.github/PULL_REQUEST_TEMPLATE.md`; the second auto-closes issues mentioning "test". Both are irrelevant to a
personal fork and safe to delete. `README.md`, `_pages/terms.md`, and the PR templates are likewise upstream docs.
