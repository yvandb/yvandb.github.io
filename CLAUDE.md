# CLAUDE.md

Not a codebase. **Trois Etoiles** is Yvan's personal recipe library, published as a Jekyll site on
GitHub Pages. No build, no tests, no linter, no package manager. You edit Markdown, GitHub renders it
on push. `_config.yml` is the theme plus an `exclude:` list keeping `raw/` and `docs/` off the live
site. Nothing else goes in it.

The job here is **curation, not engineering**. Import sources, distill them into terse recipes, keep
the index honest.

## Who reads these files

One person. He cooks constantly and has done for decades.

- 🟥 **He does not need a tutorial.** Never explain separating eggs, creaming butter, folding, peeling,
  deglazing, resting meat. If the recipe needs yolks the table says `3 egg yolks` and that is the end
  of it.
- 🟥 **He needs the specifics that decide the outcome.** Temperatures, times, ingredient temperature,
  thicknesses, doneness cues, the one unobvious move. That is the entire value of the file.
- **A recipe is a reminder, not an instruction manual.** He has either cooked it or watched the source.
  Write for the second cook, not the first.

## Layout

```
yvandb.github.io/
  README.md          # the index. landing page and source of truth for what is published
  mama.html          # standalone page, not a recipe, not indexed
  vegetables/        # 11   veg-forward dishes, salads, sides that are not mains
  fish/              #  7   fish and seafood mains
  meat/              # 11   meat mains, rubs
  sauce/             #  6   sauces, vinaigrettes, dressings
  dish/              #  8   composed mains fitting none of the above, savory tarts, pasta, dumplings
  bread/             # 21   yeasted breads, pizza dough, sandwich breads
  baking/            # 32   all sweets. pastry, cookies, cakes, tarts, waffles, custards
  resto/             #  1   restaurant notes, not recipes
  raw/               # captured sources, verbatim. committed, NOT indexed in README
    youtube/
    web/
  docs/
    TODO.md          # open items, with a status line each
    log.md           # what changed, current month only
    log/YYYY-MM.md   # archived months
  .claude/skills/    # fetch-recipe-source, add-recipe
```

## Category routing

- **Sweet tart goes to `baking/`. Savory tart goes to `dish/`** (`dish/tarte_tatin_de_tomates.md`).
- **Every dessert goes to `baking/`**, including tarts, custards and dessert waffles. `bread/` is for
  bread, which means yeast and a loaf.
- **A vegetable main is still a main.** `vegetables/` is sides and salads.
- 🟥 **Never create a new category directory.** If nothing fits, ask.
- **A stated category always wins** over your inference.

## Recipe file anatomy

```markdown
# Recipe Title

[Source or author](url) · [transcript](../raw/youtube/<capture>.md)

## Component

q | ingredient
--- | ---
250 g | plain flour

- step
- step

## 2026-04-19
- note from a cook
```

**Title.** The dish name. Never the video's clickbait headline. `This Is The Tart I Had To Perfect`
becomes `# Pear Bourdaloue`.

**Source line.** One or two links right under the title. Where it came from. If there is a capture in
`raw/`, link that too as a relative path, that is what lets the recipe stay this terse.

**Filename.** snake_case ASCII, accents stripped, `.md`. `Pâte Sucrée` becomes `pate_sucree.md`.

**Structure follows the recipe, not a template.** Judgment call every time.

- **One process, one table, one bullet list.** Do not carve up a simple recipe for symmetry.
- **Real sub-preparations each get a `##` section** with their own table and their own steps. A pear
  tart is pastry, poached pears, frangipane, assembly, glaze. Five tables beat one table with five
  blank-line groups. See `baking/pear_bourdaloue.md`.
- **Order the sections the way you would work.** Whatever chills or infuses first goes first.

**Tables.** `q | ingredient`, quantity on the left, is the house style and the most common in the repo.
`Ingredient | g` also appears and is fine where it already exists.

- **Both unit systems when the source gives both.** American and metric.
- 🟥 **Never invent a conversion.** One column is fine if that is all the source gave.
- **`bread/` and `baking/` take a baker's percent column** when the source supports it. See
  `bread/baguette.md` for the `pct | ingredient | g` convention.

**Steps.** Bullets. Fragments, not sentences.

| Keep | Cut |
|---|---|
| Oven and cooking temps, fan or not | "Preheat the oven" |
| Bake, poach, proof, rest, chill times | "Season to taste" |
| Ingredient temperature when it decides the result (soft butter, room-temp eggs) and why, in four words | "Mix until combined" |
| Doneness cues: "knife meets no resistance", "risen and deep golden" | Any basic technique |
| Thickness, size, count: "roll 2-3 mm", "slice 5 mm on an angle" | Backstory, chatter, plating opinions |
| The unobvious move: cartouche to submerge, no blind bake, fill halfway, glaze hot | Anything true of every recipe in the category |

- **4 to 8 bullets per section is the target.** Past ten, you are explaining basics.

**Notes from past cooks.** Dated headings, `## 2026-04-19` or `### 2026-04-19`, both exist. What
worked, what to change, variations.

- 🟥 **These are Yvan's alone. Never write one, never pre-fill one, never invent one.** They appear
  after he cooks it.

## Voice

Terse, telegraphic, mixed English, Dutch and French, written for one reader.

- 🟥 **Do not expand shorthand. Do not add explanatory prose. Do not tidy a table into paragraphs.**
- **No intro paragraph, no editorializing, no "enjoy!".**
- **Match the neighbours** in the same directory over any rule in this file.
- 🟨 **Older files are inconsistent and that is fine.** Do not sweep the repo normalising them. Fix
  what you touch, leave the rest.

## README is the index

🟥 **A recipe file with no README bullet does not exist.** That is the only navigation there is.

- **Add `- [Title](category/filename.md)`** to the matching section on every new recipe.
- **Sections are alphabetical**, case-insensitive, accents and leading articles ignored. Read the
  neighbours and insert in place. Never append blindly.
- 🟨 **Ordering is already imperfect.** `Pâte Sablée` sits after `Pasteurized Eggs`, and Baking has a
  nested `Wafels` sub-list plus `Yogurt cake` orphaned below it. Insert sensibly, do not re-sort the
  world unless asked.
- 🟥 **Never index anything under `raw/`.**

## raw/ is the evidence

A recipe is a lossy summary. The capture is the original, kept so the detail that did not make the cut
stays one click away and nothing needs re-fetching.

- **Committed, linked from recipes, absent from `README.md`, excluded from the live site** by
  `_config.yml`. Visible on GitHub only.
- 🟥 **Immutable. Verbatim. Never summarize, trim or tidy a capture.**
- **Dedup before fetching:** `grep -rl "<video_id>" raw/`.
- **Dashes in a capture stay.** They are the author's words, which is the one exception to the rule
  below.

## TODO and log

`docs/TODO.md` is the backlog. `docs/log.md` is the record. Neither is indexed in `README.md`.

- **TODO holds three horizons:** `## Next up`, `## Planned`, `## Someday`. An item is
  `### Title (added YYYY-MM-DD)`, a few lines of why and what it takes, then a `**Status:**` line with
  a date. Statuses: `open`, `in progress`, `blocked`, `deferred`, `needs check`.
- 🟥 **No Done section.** When an item is finished, delete it from TODO and append to the log.
  `README.md` already is the list of what shipped.
- ✅ **File it rather than dropping it.** A flagged uncertainty, an unverified conversion, a recipe
  imported but not yet cooked, a repo chore: these go in TODO with a status, not into a chat reply
  that scrolls away.
- **Log entries are one per session or batch**, `## [YYYY-MM-DD] short subject` then a line per change,
  appended at the bottom. Current month only, roll the old month into `docs/log/YYYY-MM.md` when a new
  one starts.
- 🟥 **Cooking a recipe is not a log entry.** That is a dated note inside the recipe file, and it is
  Yvan's to write.
- **Update both when you finish work here**, same commit as the work.

## Skills

- **`fetch-recipe-source <url>`** captures a YouTube video or web page into `raw/`. Writes the link,
  the full description and the full transcript. Writes no recipe, touches no README.
  - **Always capture the YouTube description.** That is where the exact grams live. The spoken
    transcript almost never states full quantities.
- **`add-recipe <raw path | url>`** converts a capture into a recipe and updates `README.md`. Given a
  URL it runs the fetch skill first.

## Git

Per `~/.claude/CLAUDE.md`: everything on `master`, no branches, no PRs. Push regularly, not after every commit. A push publishes the live site. Account `yvandb`, remote `git@github-personal:yvandb/yvandb.github.io.git`.

## Dashes

🟥 **Never emit an em dash or an en dash.** Not in a recipe, not in a commit message, not in a reply.
Period, comma, colon, parentheses or a plain hyphen instead. A plain `-` is fine, `2-3 mm` is fine.

- ✅ **Verbatim source text keeps its dashes.** Captures in `raw/` are verbatim.
- 🟥 **Check with `grep -nE '—|–|&mdash;|&ndash;' <files you wrote>` before finishing.** Must come back
  empty. Never `grep -rn $'—\|–'`, zsh eats the backslash and it silently passes every time.

## Never

- **Invent a quantity, a conversion, a substitution or a tip the source did not give.** A missing
  number gets flagged loudly, not filled in.
- **Write a dated note entry.**
- **Create a category.**
- **Index `raw/` or `docs/` in the README.**
- **Re-fetch a source already in `raw/`.**
- **Add to `_config.yml` beyond the theme and the exclude list, add a Gemfile, add tooling, or turn
  this into a project.**
