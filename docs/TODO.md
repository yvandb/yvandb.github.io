# TODO

Open items: recipes to import, recipes to cook, things in the repo that need fixing. Not a commitment,
a place so nothing is lost. Newest near the top of each section.

**Lifecycle.** An item lands in a horizon section with a `**Status:**` line. When it is done, drop it
from here and append a line to `docs/log.md`. There is no "Done" section, `README.md` is the list of
what shipped.

**Item shape.** `### Title (added YYYY-MM-DD)`, a few lines of why and what it takes, then
`**Status:**` with a date. Statuses in use: `open`, `in progress`, `blocked`, `deferred`, `needs check`.

## Next up

### Cook the pear bourdaloue (added 2026-09-27)
`baking/pear_bourdaloue.md` is imported but untested. First cook confirms the frangipane ratio and
whether 175C fan for 40 min is right in this oven. Note goes under a dated heading in the recipe.

**Status:** open, 2026-09-27

### Verify the metric on the banana oat biscuits (added 2026-09-27)
`baking/banana_oat_breakfast_biscuits.md` carries a `gr` column that is standard conversion, not from
the source. The source was cups only. Oats at 90g/cup and whole wheat at 90g per 3/4 cup are the two
worth weighing once.

**Status:** needs check, 2026-09-27

### Commit or drop dish/haianese_chicken.md (added 2026-09-27)
Staged and modified since before 2026-09-14, never committed, never indexed in `README.md`. Either
finish it and add the bullet, or discard it.

**Status:** open, 2026-09-27

## Planned

### Exercise the web path of fetch-recipe-source (added 2026-09-27)
`raw/web/` is empty. Only the YouTube path has ever run, so the `WebFetch` branch and the web capture
template are unverified. Next non-YouTube recipe import is the test.

**Status:** open, 2026-09-27

## Someday

### Re-sort the Baking section of README.md (added 2026-09-27)
Ordering drifted. `Pâte Sablée` and `Pâte Sucrée` sit after `Pasteurized Eggs` instead of before
`Red Velvet`, `Sandies` is above `Red Velvet`, the `Wafels` sub-list breaks the flat alphabetical run,
and `Yogurt cake` is orphaned below it. Cosmetic, and re-sorting churns the diff, so it waits for a
reason.

**Status:** deferred, 2026-09-27
