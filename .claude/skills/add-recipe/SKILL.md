---
name: add-recipe
description: Turn a raw capture in raw/ (or a URL, which gets captured first via fetch-recipe-source) into a recipe Markdown file in the right category, and update README.md. Use when asked to add, import, or convert a recipe.
allowed-tools: Bash, Read, Write, Edit, Glob, Grep, WebFetch, Skill
---

# add-recipe

Convert a source into a recipe file in this cookbook, then index it in `README.md`.

## Inputs

`$ARGUMENTS` is one of:

- **A path in `raw/`** - convert that capture. `/add-recipe raw/youtube/2026-09-04-pear-bourdaloue-H8owaQbV1ok.md`
- **A URL** - run `fetch-recipe-source` on it first, then convert the capture it wrote. `/add-recipe https://www.youtube.com/watch?v=H8owaQbV1ok`
- **Nothing** - list the captures in `raw/` that have no recipe pointing back at them, and ask which one.

An optional trailing hint overrides the category or filename: `/add-recipe <url> baking as "pear_bourdaloue"`.

## Steps

### 1. Get the capture

If the argument is a URL, invoke the `fetch-recipe-source` skill on it and wait for the file it writes. If that skill reports the source is already captured, use the existing path instead of re-fetching.

Read the capture in full. The description block usually holds exact quantities; the transcript holds the technique and the numbers the description leaves out. Use both. Where they disagree, the transcript wins on method, the description wins on quantities, and you say so in the recipe if the gap matters.

### 2. Pick the category

| Category | Directory | What goes there |
|---|---|---|
| Vegetables | `vegetables/` | Vegetable-forward dishes, salads, sides that aren't mains |
| Fish | `fish/` | Fish and seafood mains |
| Meat | `meat/` | Meat mains (beef, pork, poultry, lamb, game), rubs |
| Sauce | `sauce/` | Sauces, vinaigrettes, dressings |
| Dish | `dish/` | Composed mains that fit none of fish/meat/vegetable (dumplings, stuffed cabbage, pasta, tofu mains, savory tarts) |
| Bread | `bread/` | Yeasted breads, pizza dough, sandwich breads |
| Baking | `baking/` | Sweets, pastries, cookies, cakes, tarts, waffles, custards. **All desserts go here** |
| Restaurants | `resto/` | Restaurant notes, not recipes |

Edge cases:

- Sweet tart -> `baking/`. Savory tart -> `dish/` (see `dish/tarte_tatin_de_tomates.md`).
- Dessert waffle -> `baking/`. Bread-like breakfast item -> `bread/`.
- A user-supplied hint always wins.
- If two categories are genuinely defensible, ask. Don't create a new category.

### 3. Filename

snake_case ASCII, `.md`. Strip accents (é -> e, û -> u), drop punctuation, spaces to underscores. "Pâte Sucrée" -> `pate_sucree.md`. If the file already exists, ask before overwriting.

### 4. Write the recipe

```markdown
# <Recipe Title>

[<Author or site>](<original URL>) · [transcript](../raw/youtube/<capture>.md)

## <Component>

q | ingredient
--- | ---
250 g | plain flour
...

- step
- step

## <Next component>
...
```

**Title.** The dish name, not the video's clickbait headline. "This Is The Tart I Had To Perfect" -> `# Pear Bourdaloue`.

**Links.** Two, on one line under the title: the original source, and the raw capture as a relative path (`../raw/youtube/<file>.md`). The capture link is what makes the terse recipe safe to be terse. If there is no capture (recipe typed from a book), just the source line, or nothing.

**Structure follows the recipe, not a template.** Judgment call each time:

- A single-process recipe gets one ingredient table and one bullet list. Don't split it up for the sake of it.
- A recipe with real sub-preparations gets a `##` section per sub-preparation, each with its own table and its own steps. A pear tart is pastry, poached pears, frangipane, assembly and bake, finish. Five tables beats one table with five blank-line groups.
- Order the sections the way you'd actually work: the thing that needs to chill or infuse first goes first.

**Ingredient tables.** `q | ingredient`, quantity on the left. Copy the source's units verbatim. Metric alone is fine if that's all the source gave; imperial alone likewise. **Do not invent conversions.** For bread and baking where the source gives a baker's percent, add the column (see `bread/baguette.md`).

**Steps: high signal only.** This is the whole point. He cooks, he does not need a tutorial.

Keep, always:

- Temperatures and times. Oven temp with fan/no-fan if stated, bake time, poach time, rest and chill times, proof times.
- Ingredient temperature when it decides the outcome. Soft butter vs cold butter, room-temp eggs. Say why in four words ("or the frangipane splits"), not four sentences.
- Doneness and visual cues. "knife goes in with no resistance", "risen and deep golden".
- Thicknesses, sizes, counts. "roll 2-3 mm", "slice 5 mm on an angle", "8 pear halves".
- The specific, unobvious move. Cartouche to keep pears submerged. No blind bake. Fill the case only halfway. Glaze hot. These are the reason to keep the recipe at all.

Cut, always:

- Basic technique. Don't explain separating eggs, creaming, folding, peeling. If the recipe needs yolks, the table says "3 egg yolks" and that is the end of it.
- Generic instructions. "Preheat the oven." "Season to taste." "Mix until combined."
- Chatter, backstory, plating opinions, brand endorsements.
- Anything true of every recipe in this category.

A good target is 4-8 bullets per section. If a section runs past ten, you are explaining basics.

**Voice.** Terse. Fragments over sentences. No intro paragraph, no editorializing, no "enjoy!". Match the neighbouring files in the category.

**No notes section.** `### YYYY-MM-DD` entries are Yvan's own, written after he cooks it. Never pre-fill one.

### 5. Update README.md

Add one bullet to the matching section:

```markdown
- [<Title>](<category>/<filename>.md)
```

Sections are alphabetical, case-insensitive, ignoring accents and leading articles. Read the existing bullets and insert at the right point. Do not append blindly. Do not index anything under `raw/`.

### 6. Report

Category, file path, README section touched, and anything you had to guess at. Flag a guessed quantity or a missing temperature loudly. One short block.

## What not to do

- Don't index `raw/` in `README.md`.
- Don't add a dated notes section.
- Don't invent quantities, conversions, substitutions or tips the source didn't give. A missing number gets flagged, not filled in.
- Don't create a new category directory.
- Don't re-fetch a source already captured in `raw/`.
