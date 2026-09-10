# 3D printing files

Drop STL / 3MF / STEP / OBJ files for the 3D-printed device work in here.

## This directory is public

`files` is listed under `include:` in `_config.yml`, so anything in here is copied into the built site and served
from `https://joeyschulz.com/files/3d-printing/<name>` as soon as it is pushed. There is no draft or staging state.
Before adding a file:

- **Patient-specific geometry.** The electron cutout shapes are derived from patient anatomy. Publish phantom or
  generic versions — not anything traceable to a patient.
- **IP and licensing.** Check whether the device is covered by a licence or disclosure before publishing its
  geometry.
- **Metadata rides along.** Slicer projects (`.3mf`, `.gcode`) and CAD files can embed absolute file paths, printer
  profiles, timestamps, and usernames. Export a clean mesh rather than saving a working project.

If you want to stage files locally without publishing them, add this folder to `.gitignore` first.

## Naming

`<device>-<variant>.<ext>`, lower case, hyphens, no spaces — for example:

```
electron-cutout-6x6.stl
photon-block-lung.3mf
```

Spaces, `#`, and `?` in filenames break the resulting URL. A filename starting with `_` is silently ignored by
Jekyll and never reaches the built site — no error, the file just is not there.

## Where these get used

Nothing links here yet — see the 3D printing section in `TODO.md` at the repo root for the open decisions.

(This README is in `exclude:` in `_config.yml`, so it is not published alongside the files.)
