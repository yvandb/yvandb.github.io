# Log

What actually changed. **This file holds the current month (2026-11) only.** Entries append at the
bottom, newest last. Earlier months archive to [`log/`](log/) as `YYYY-MM.md`.

One entry per session or per batch of work, not per file. Format:

```
## [YYYY-MM-DD] short subject
- what changed, in a line each
```

Cooking a recipe is not a log entry, that is a dated note inside the recipe.

## [2026-11-22] vanilla pudding, ultra-soft

- Added `baking/vanilla_pudding.md`, the 700 g milk version, from text pasted into the session.
- Capture in `raw/web/2026-11-22-ultra-soft-vanilla-pudding-pasted.md`, verbatim. No source link.
- Rolled October into `docs/log/2026-10.md`.

## [2026-11-22] hainanese chicken

- Committed `dish/hainanese_chicken.md`, staged since before 2026-09-14. Renamed from
  `dish/haianese_chicken.md`, title spelling fixed.
- Fixed the source link, url and text were swapped. Dropped an empty `[]()` line.
- Indexed in `README.md`. Dropped its TODO item.

## [2026-11-22] what a recipe needs

- `CLAUDE.md`: the core is ingredient balance plus cooking stages. Serving details (ramekin size,
  chill time, portions) are not gaps, never flagged.
- Pudding TODO trimmed to the one open question: does 700 g milk set.

## [2026-11-22] noindex

- `_includes/head-custom.html` puts `<meta name="robots" content="noindex, nofollow">` in every page
  the modernist theme renders. Same tag added to `mama.html` by hand.
- No `robots.txt` on purpose: blocking the crawl stops search engines from seeing the noindex.
