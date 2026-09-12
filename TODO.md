# TODO

Working list for the site. Not part of the build — `TODO.md` is in `exclude:` in `_config.yml`, so GitHub Pages does
not turn it into a public page (see the `jekyll-optional-front-matter` note in CLAUDE.md).

## 3D printing files

Drop the source files in **`files/3d-printing/`** — that folder's README has the naming convention and the
publishing caveat.

- [ ] Electron cutouts —
- [ ] Photon blocks —
- [ ] Anything else worth showing —

Then, per file:

- [ ] **Which of these are OK to publish?** The repo is public and everything under `files/` is copied into the built
      site, so a file dropped in there is live at `joeyschulz.com/files/3d-printing/<name>` the moment it is pushed.
      There is no draft state.
- [ ] **Is any of it patient-specific?** The electron cutout geometry comes from patient anatomy — those want to be
      phantom or generic versions, or to stay out of the repo entirely.
- [ ] **Any IP or licensing strings?** Worth checking before the non-toxic photon block files go up, given the AVATAR
      2.0 licence went to Leo Cancer Care.
- [ ] **Where do they surface?** Options: download links on the existing `_portfolio` entries, a new `/projects/`
      demo (an in-browser STL viewer would fit the no-external-requests house rule), or just the bare folder.

## Known gaps

- [ ] **Brainrot gutter clip** — the panel is wired up but the file is not there yet. Drop the loop at
      `images/brainrot/subway-surfers.gif` (or rename `gif:` in `_data/brainrot.yml` to match whatever you drop in).
      Until then the panel removes itself on the 404 and nothing changes.
- [ ] ORCID iD for the sidebar — `author.orcid` in `_config.yml` is still blank
- [ ] `files/cv.pdf` for the CV download link
- [ ] Real talk dates — every `_talks` file uses `YYYY-01-01` because the CV only gives years
- [ ] Paper figures — 14 of 17 publications have no figure in `images/publications/`; the three that do are the
      Frontiers (CC BY) papers. Which figure is the main one is your editorial call.
- [ ] `/cv-json/` is half-populated — `scripts/cv_markdown_to_json.py` wants a rigid `Degree, Institution, YYYY`
      bullet format the real CV does not use. Fix the script or delete the page.
