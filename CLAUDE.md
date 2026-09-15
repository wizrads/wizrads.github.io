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

**Always preview with `--config _config.yml,_config_docker.yml`.** `url` is `https://joeyschulz.com` and every
asset link is built from it, so a preview without that override silently loads the *live* CSS and JS and your local
changes appear to do nothing. **`jekyll serve` on its own does NOT rewrite `url` here** — it only does that when
`url` is blank in the config, and this one is not. `_config_docker.yml` is a one-line `url: ""`, which is exactly
that override; `.claude/launch.json` passes it. Confirm before trusting a preview:

```bash
curl -s http://localhost:4000/ | grep -o 'href="[^"]*main\.css"'   # must be /assets/..., not https://joeyschulz.com/...
```

If you do end up checking against live assets, the fallback is to prove they match what you built:
`shasum -a 256 _site/assets/css/main.css` against `curl -s https://joeyschulz.com/assets/css/main.css | shasum -a 256`.

**JavaScript is committed pre-built.** `main.min.js` is jquery + `plugins/jquery.greedy-navigation.js` + `_main.js` +
`theme.js` uglified together, and it is the only script the site loads. After editing any of those sources, run
`npm run build:js` and commit the regenerated bundle.

There is no test suite. CI is `.github/workflows/jekyll-build.yml`, but it triggers on branch `main` while this repo
uses `master`, so it never actually runs. Verify builds locally.

**Production compresses the HTML; development does not — so test inline JavaScript against a production build.**
`layout: compress` runs on every page and `compress_html.ignore.envs` is `development`, which is what `jekyll serve`
sets. It used to collapse every whitespace run outside `<pre>` (Liquid's `split: " "` is Ruby's whitespace-run split,
so **newlines went too**), which silently turned any `//` comment in an inline `<script>` into one that swallowed the
rest of the file — the chart script in the blog post shipped as `SyntaxError: Unexpected end of input`, so nothing on
the page ran, while `jekyll serve` looked perfect. `compress_html.blanklines: true` in `_config.yml` now keeps the
newlines; **do not remove it**, and prefer `/* */` in inline scripts anyway.

Checking that a production build's JSON still parses is not enough — the script has to run. Serve the compressed
output and load it:

```bash
JEKYLL_ENV=production bundle exec jekyll serve -H localhost -P 4001 --config _config.yml,_config_docker.yml --no-watch
```

A quick syntax check of any page's inline script, local or live, is worth doing too — extract between the last
`<script>` and its `</script>` and run `node --check` on it.

**A green local build does not guarantee a green Pages build.** GitHub Pages force-enables
`jekyll-optional-front-matter`, which turns *every* front-matter-less `.md` file in the repo into a page and runs it
through Liquid — local Jekyll does not, and copies those files verbatim. `CLAUDE.md` quotes `{{` and `{%`, so Pages
choked on it and the deploy failed while `jekyll build` stayed clean; it is now in `exclude:` in `_config.yml`. Any
new Markdown at the repo root needs front matter, an `exclude:` entry, or no Liquid braces. If a push does not show
up on the live site, check `gh run list --repo wizrads/wizrads.github.io`, not just the build status.

**Do not trust the commit SHA either endpoint reports.** Pages builds the branch *tip at run time*, so a run
triggered by one commit happily deploys a later one while still labelling itself with the older SHA - push twice in
quick succession and the second commit deploys with no run of its own, and `pages/builds/latest` keeps naming the
first. Verify a deploy by its *content*: build locally and compare, e.g.
`curl -s https://joeyschulz.com/assets/css/main.css | shasum -a 256` against `_site/assets/css/main.css`.

## Content

All template placeholder content is gone. `_config.yml`, `_pages/about.md`, `_pages/cv.md`, and the `_publications`
(17), `_talks` (28), `_teaching` (2), and `_portfolio` (4) collections hold real content generated from Joey's CV.
`_posts/` holds one post (see **The blog** below). The nav is Blog / Publications / Talks / Teaching / Portfolio /
Projects.

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

The homepage and each blog post can carry a joke switch that swaps the copy for a Gen Alpha translation. Four
moving parts:

- `_data/brainrot.yml` — the homepage's alternate copy: `page:` (keyed by block), `nav:` (keyed by rendered nav
  label), `bio:`. Values are injected as HTML, so a translation must carry over any links the real copy had.
  `gifs:` is the odd one out: the list of looping clips described below, not copy. `nav:`, `bio:` and `gifs:` are
  **site furniture** — every page with a toggle gets them from this file, whatever its own copy is.
- `_pages/about.md` — each translatable block is tagged with a kramdown inline attribute list,
  `{: data-brainrot="intro-lab"}` on the line after it. `{:` is not Liquid, so it survives the build; the IAL does
  not disturb the heading ids `auto_ids` generates. The bullets are tagged as **one** list (`research-list`), so
  that translation supplies all four `<li>` elements.
- `_includes/brainrot-toggle.html` — the switches plus `<script type="application/json" id="brainrot-data">`. It takes
  `copy=` (the block of page text to use, defaulting to `site.data.brainrot`), an optional `hint=`, and `gifs=`, and
  hand-builds that JSON as `{page: copy.page, nav: …, bio: …}` rather than jsonifying one file, which is what lets a
  post supply its own `page:` while still sharing the nav and bio. `about.md` includes it itself, at the top of the
  file. **Each switch is wrapped in its own `.brainrot__switch`**, because the checked styles are sibling selectors
  off the input and two inputs under one parent light up every label in it.
- `setupBrainrot()` in `assets/js/_main.js` (styles in `_sass/include/_brainrot.scss`) — reads that JSON, stores each
  element's real `innerHTML`, and swaps. It returns immediately when the include is absent, so every other page is
  untouched. The state persists in `localStorage.brainrot`, so the toggle stays on across pages that have one.

**Giving a blog post a slider is one file.** `_layouts/single.html` looks up
`site.data.brainrot_posts[page.slug]`, and where that file exists it renders the toggle at the top of
`.page__content` and passes the file in as `copy=`. So: add `_data/brainrot_posts/<post-slug>.yml` with a `page:`
map, tag the post's blocks with `data-brainrot="key"` in their opening tags, and that is the whole wiring — nothing
in the post's front matter, no include, no JS. `posttitle` is the one reserved key: the layout puts
`data-brainrot="posttitle"` on the `<h1>` only when the file defines it, so the front-matter title translates too.
`hint:` at the top level of the file replaces the toggle's "translate this page into Gen Alpha" line, and `gifs: true`
opts the post into the gutter clips (see below — they are **off** on posts otherwise). The lookup is by slug, so it
works for any page on the `single` layout, not only posts.

`_data/brainrot_posts/ask-eight-llms.yml` is the worked example, and its register is the one to match: all lowercase,
heavy slang, several emoji a paragraph. The homepage's copy in `_data/brainrot.yml` is milder — it was written first
and has not been brought up to the same level.

Translate prose, never a quotation. `_data/brainrot_posts/ask-eight-llms.yml` deliberately leaves the prompt box,
Grok's refusal, the model tables and every `figcaption` alone — those are records of what was actually asked and
answered, and a punchier wording would make them wrong. Same rule as the attribution one below: carry every number
through unchanged.

A block whose text is rewritten but whose key is left in place still renders fine — it just stops translating, so
**editing the real copy can never break the page**. Keep the two in sync by hand. Pages with no data file are
untouched, so the nav labels revert when you navigate to one.

**The `nav:` translations have a width budget.** They are swapped in place, and a nav wide enough to wrap the site
title grows the masthead past the `$masthead-height` the stylesheet reserves — the padding jump described below. Keep
each translated label about as wide as the label it replaces (the emoji sit flush against the word for this reason:
the space costs ~5px each; Talks and Teaching became "Yaps" and "Teach" to pay for the Blog tab) and check the total,
which must stay at or under the plain nav's:
`document.querySelector("#site-nav .visible-links").getBoundingClientRect().width`. `renderBrainrot()` then triggers
a `resize` on the **next animation frame** so the greedy nav re-measures a settled layout; triggering it in the same
frame reads a stale masthead height and leaves the body padding a few pixels off.

Emoji are plain characters in the YAML, rendered by the system emoji font, so none of them costs a request.

**The gutter clips.** `gifs:` in `_data/brainrot.yml` lists looping files in the repo — currently Subway Surfers, a
cat, and the Rizzler — stacked down the right of the page while the toggle is on, the gameplay half of a brainrot
TikTok. Each entry takes `src` (site-relative path) and an optional `width` (default 520px). The
`<aside id="brainrot-gif">` renders only when the list is non-empty, `renderBrainrot()` shows and hides it, and each
`src` is copied from `data-src` the *first* time a clip is actually shown (and the `data-src` cleared, which is what
makes that run once), so nothing is fetched for anyone who never turns them on.

**They are on for the homepage and off for blog posts.** The include's `gifs=` defaults to on, which is what
`about.md` gets; `single.html` passes `false` unless the post's data file sets `gifs: true`. Both ends compare
against `false` **explicitly** rather than using Liquid's `default` filter, which treats `false` as "not set" and
would switch them straight back on. With `gifs=false` neither the panel nor its switch is rendered at all.

**And each viewer can switch them off for themselves.** A second toggle, `clips 🎬`, sits beside the brainrot one
wherever the panel exists, and `renderBrainrot()` reveals it only while brainrot mode is on. The preference is
`localStorage.brainrotGifs`, so it is per browser: switching the clips off changes nothing for anyone else, and the
clip files are never fetched while it is off. Note `.brainrot__switch[hidden] { display: none }` in
`_sass/include/_brainrot.scss` — the same trap as the panel below, since `.brainrot__switch`'s own `display:
inline-flex` beats the browser's `[hidden]` rule and the switch would otherwise show with brainrot mode off. There is no `error` handling: a
`src` naming a file that is not there shows a broken-image icon, so keep the list and `images/brainrot/` in sync.

The panel is `position: fixed` in the **top right**, below the fixed masthead (`top: $masthead-height + 0.75em`, so
it follows that constant if the masthead ever changes height) and at `z-index: 10`, under the masthead's 20. The
clips are deliberately wider than the gutter and cover the text — `pointer-events: none` on the panel is what keeps
that purely visual, so links underneath still work. They show at **every width**; `prefers-reduced-motion` is the
only thing that suppresses them, since a loop the viewer cannot pause is the entire point.

**Size each clip with `max-width` and `max-height` only — never `width` or `height`.** Two constraints and no fixed
dimension is what makes the browser scale each one by its own aspect ratio, so the element *is* the picture. Setting
a width instead leaves a shrunken image letterboxed inside a wider box, which is what the drop shadow and rounded
corners used to frame (both now gone, along with the `cutout:` flag that existed to opt out of them). `max-width` is
inline per clip, written `min(<width>, 42vw)` in the include because the stylesheet cannot also set that property and
libsass eats `min()` anyway; `max-height` is an equal share of the window, `(100vh - masthead) / var(--brainrot-count)`,
with the count emitted onto the `<aside>` by Liquid. That is what keeps the column inside a viewport nobody can
scroll — it is `position: fixed`.

**The panel needs `display: none` on `.brainrot-gif` *and* `display: flex` on `.brainrot-gif:not([hidden])`.**
Neither alone works, and both failures have shipped. The `hidden` attribute the JS sets is inert here, because the
HTML5 reset in `_sass/layout/_reset.scss` sets `aside { display: block }` and any author rule beats the browser's own
`[hidden] { display: none }` — so with no `display: none` the clips keep playing after the toggle goes off. Put the
visible `display` on the bare selector instead and they play for everyone who never touched the toggle.

`images/brainrot/cat.gif` was a 1000×1000 green-screen GIF, 8.3MB. It was resized to 300px and had the green keyed
out to real GIF transparency frame by frame with Pillow (a throwaway script, not kept: convert each frame to RGBA,
zero the alpha where `g > 90 && g > 1.35r && g > 1.35b`, quantize to 127 colours, reserve index 255 as the
transparency index, save with `disposal=2`). That is what `cutout: true` exists for. Do the same to any other
green-screen clip rather than shipping the green.

**Attribution still applies here.** The `background-stanford` translation keeps the "team projects I was part of"
framing for the 3D-printed devices and AVATAR 2.0 (see **Content** above) — do not let a punchier rewrite turn those
into things he led.

### The blog

`_posts/` has one post, `2026-09-14-ask-eight-llms.md`, served at **`/ask-eight-llms/`** — posts take
`permalink: /:categories/:title/` from `_config.yml`, and it declares no categories. `_pages/year-archive.html`
is the listing page and the **Blog** nav tab; it serves at **`/blog/`** and redirects `/year-archive/` there (the
file keeps its upstream name).

It arrived as a single self-contained 46KB HTML file (five hand-written SVG charts, an inline JSON data block, and
~260 lines of vanilla JS) and was moved in without touching the charts. What that required, and what to repeat for
the next one:

- **Scope the CSS.** The original styled `:root` and bare `body`/`p`/`h2`/`figure`/`table`/`details` selectors, which
  would fight the theme in both directions. Everything is wrapped in `<div class="llm-post">`, the `:root` variables
  moved onto `.llm-post`, and every selector prefixed. The `<style>` lives inline in the post body, so it loads on
  that page only and wins ties against `main.css` on source order — no `!important` anywhere.
- **Undo what the theme paints.** Prefixing is not enough on its own, because the theme still reaches in through its
  own selectors. A short reset block near the top of the post's stylesheet handles the four that actually broke
  things: `figure{display:block}` (the theme makes figures flex, which scatters the chart parts across a row),
  `figcaption{font-family:inherit;margin-bottom:0}`, `table{border:0}` + `thead{background-color:transparent}` +
  `th,td{border-right:0}`, and `h2{padding-bottom:0;border-bottom:0}`.
- **One title only.** The layout renders the front-matter title, so the in-body `<h1>` and its `.eyebrow` were
  deleted and `header` lost its big top padding. The `.lede` and the conflict-of-interest callout stayed.
- **Set `excerpt:` explicitly.** Without it the archive listing and the RSS summary take the first block, which here
  is the opening `<div>` and the stylesheet.
- **`feed: excerpt_only: true`.** jekyll-feed inlines full post content; for this post that meant a `<style>` block
  and 260 lines of chart code in `feed.xml` (49KB) that no reader can run. The flag is read per-post at
  `post.feed.excerpt_only`.
- **Liquid.** The file had no `{{` or `{%`, so it passes through untouched. Check with `grep -c` after any edit and
  wrap the script in `{% raw %}` if that ever changes.
- **Web fonts are opt-in per page.** `_includes/head/custom.html` emits a stylesheet link only when a page sets
  `google_fonts:` in its front matter, so the rest of the site still makes no request to Google.
- **Give it a brainrot slider.** Tag the post's prose in the opening tags (`<p data-brainrot="worth-1">`) and add
  `_data/brainrot_posts/<slug>.yml`; the toggle then appears under the title on its own. See **Brainrot mode**
  above. The swap is `innerHTML` on the tagged elements only, so nothing the chart script draws into (`#c-*`,
  `#t-*`, `#lineup`, `#settings`) may be tagged — and nothing here listens for `resize`, which
  `renderBrainrot()` fires, so the charts survive the toggle untouched. Verified by clicking it in a production
  build: five SVGs before, during and after.

Known rough edges, both judgement calls rather than bugs:

- **The charts are drawn for a ~1080px column and get ~670px** inside `single` with the author sidebar, so their
  12px SVG labels render around 9–10px. Legible, checked, but smaller than designed. `author_profile: false` on the
  post would buy the width back at the cost of the sidebar; the theme has no `classes: wide` support.
- **The post is light-only on purpose** (`color-scheme: light only`), but the site *does* have a dark toggle. Flip
  it and the post stays a white sheet inside a dark page. Adding dark variants was explicitly declined.

### Hidden pages

`/cv/` is deliberately hidden but not deleted: its nav entry is commented out in `_data/navigation.yml` and it carries
`sitemap: false`. Note that flag needs **two** cooperating pieces — `jekyll-sitemap` reads it for `sitemap.xml`, and
`_pages/sitemap.md` was patched to honour it as well, because that human-readable sitemap is linked from the footer of
every page and would otherwise keep the page one click away. Use the same pattern for anything else to be hidden.

`/cv-json/` (`_pages/cv-json.md` + `_data/cv.json`) is unlinked, still in `sitemap.xml`, and only partly populated:
`scripts/cv_markdown_to_json.py` demands a rigid `Degree, Institution, YYYY` bullet format that the real CV does not
fit, so its Education and Work sections come out empty. Either fix the script or delete the page — do not contort
`cv.md` to satisfy the parser.

Known gaps are tracked in **`TODO.md`** at the repo root (excluded from the build, so Pages does not publish it):
no ORCID iD in the sidebar, no `files/cv.pdf` for the download link, real talk dates outstanding, 14 of 17 paper
figures missing, `/cv-json/` half-populated.

`files/3d-printing/` is where Joey drops STL/3MF/STEP source for the 3D-printed device work; its README (also
excluded) carries the naming convention and the caveats. **Everything under `files/` is copied into the built site
and served**, so a file pushed there is public immediately — the open question of which print files are safe to
publish (patient-specific cutout geometry, licensing) is a checklist item in `TODO.md`, not something to answer on
his behalf.

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
  `preprints`), analytics/comments providers, and `site_theme`. **Analytics is on**: `provider` is
  `google-analytics-4` with Joey's GA4 measurement ID, rendered by `_includes/analytics-providers/google-analytics-4.html`
  through `analytics.html` → `scripts.html`, so it lands on every page built from a layout. A single page opts out with
  `analytics: false` in its front matter. Two things it does **not** cover, both by design: the `redirect_from` stubs
  (meta-refresh pages that bounce to a destination which is itself tracked) and `projects/<slug>/index.html`, which is
  static and carries no site chrome — tracking a demo would also break the no-external-requests house rule above. `url` must stay equal to the custom domain, since
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
- **The nav cannot overflow, so the site title wraps instead.** `.visible-links` is `display: table`, so it shrinks
  to fit rather than exceeding the nav width — which means `updateNav()`'s `$vlinks.width() > availableSpace` test
  never fires and items never move into the hamburger. What gives instead is the title, wrapping to two lines and
  taking the masthead from 65px to 92px. With six nav items that now happens in a band roughly **790–850px wide**
  (below ~768px the narrow masthead takes over and it is fine again). Adding a seventh item, or lengthening a label,
  widens that band. A real fix means changing the list's layout model (`table-layout: fixed` plus `white-space:
  nowrap` on the cells, or dropping `display: table`) and then re-measuring both height constants — `flex-wrap` does
  nothing here, the list is not a flex container.
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
  It is also in **normal document flow**, at the end of the page. Upstream had it `position: fixed; bottom: 0`,
  parked across the bottom of the window on every screen, which needed three other pieces to stay out of the way:
  `body { padding-bottom: 9em }` in `_sass/layout/_base.scss`, a `bumpIt()` function in `_main.js` that re-set the
  body's margin from the footer's measured height on a 250ms `setInterval`, and a `max-width: 768px` rule un-pinning
  it on phones. All four are gone together — if you ever re-pin the footer, they come back as a set.
- **`_sass/layout/_base.scss` + `_sass/layout/_page.scss` — the sticky footer is flexbox, not `position: fixed`.**
  `body` is `display: flex; flex-direction: column; min-height: 100vh` (then `100dvh`, which wins where supported and
  excludes mobile browser chrome), and `#main` is `flex: 1 0 auto`. A short page — `/blog/`, `/teaching/` — therefore
  pushes the footer to the bottom of the window instead of leaving it stranded mid-screen, and a long page flows past
  it unchanged. Nothing is measured at runtime, which is the whole point: this is **not** the old fixed footer coming
  back. Two things make it work and are easy to delete by accident:
  - `#main` needs its explicit `width: 100%`. Susy's `@include container` gives it `margin-left/right: auto`, and an
    auto margin on a flex container's *cross* axis suppresses stretching — without the width it collapses to fit its
    content.
  - `.masthead` and the brainrot gutter panel are `position: fixed`, so they are not flex items and are unaffected.
    Anything new added as a direct child of `body` **will** become a flex item.
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
