# Log

What actually changed. **This file holds the current month (2026-09) only.** Entries append at the
bottom, newest last. Earlier months archive to [`log/`](log/) as `YYYY-MM.md`.

One entry per session or per batch of work, not per file. Format:

```
## [YYYY-MM-DD] short subject
- what changed, in a line each
```

Cooking a recipe is not a log entry, that is a dated note inside the recipe.

## [2026-09-14] recipe import skills

- Added `fetch-recipe-source` skill: captures a YouTube video or web page into `raw/` verbatim, with
  the link, the description and the full transcript. No recipe, no README.
- Added `raw/youtube/` and `raw/web/`. Committed, linked from recipes, never indexed.
- Rewrote `add-recipe` to take a raw path or a URL, and to fetch first when given a URL.
- First import: `baking/pear_bourdaloue.md` from Matthew Ryle.

## [2026-09-14] CLAUDE.md rewrite

- Replaced the generic codebase boilerplate with what this repo actually is. Category routing, recipe
  anatomy, a keep/cut table for steps, the no-dashes rule, README as the index.

## [2026-09-19] banana oat breakfast biscuits

- Added `baking/banana_oat_breakfast_biscuits.md`, belVita soft baked copycat, from pasted text with
  no source URL so no capture in `raw/`.
- Metric column is standard conversion, not from the source. Filed in `docs/TODO.md`.

## [2026-09-27] todo and log mechanism

- Added `docs/TODO.md` and `docs/log.md`, modelled on `~/git/personal/viewer/docs/TODO.md` and the
  monthly log in `~/git/personal/art-of-ai/log.md`, scaled down to a cookbook.
- Seeded TODO with the five open items standing at the time.
- Documented both in `CLAUDE.md`.

## [2026-09-27] raw/ and docs/ off the live site

- Added `exclude: [raw, docs]` to `_config.yml`. Both stay in the repo and on GitHub, Pages no longer
  builds them.
- Updated `CLAUDE.md` to allow that one addition to `_config.yml`.
