---
name: add-recipe
description: Fetch a recipe from a URL (web page or YouTube video) and add it to this cookbook as a Markdown file in the correct category, with a link back to the source. Also updates README.md.
---

# add-recipe

Given a URL, add a new recipe to this cookbook. Works for web pages and YouTube videos (including Shorts).

## Inputs

The user will pass a URL as the argument, optionally followed by a category hint or a suggested filename. Examples:
- `/add-recipe https://www.seriouseats.com/something`
- `/add-recipe https://youtube.com/shorts/abc123 sauce`
- `/add-recipe https://youtu.be/abc123 baking as "nutella_brioche"`

If the user only passes a URL, infer the category yourself.

## Steps

### 1. Fetch the content

**Web pages**: use `WebFetch` with a prompt like "Extract the recipe: title, ingredient list with quantities in both American and metric units if available, and the step-by-step instructions. Preserve numbers and units exactly."

**YouTube URLs** (`youtube.com/watch`, `youtu.be/`, `youtube.com/shorts/`): get the transcript.
- Preferred: `yt-dlp --write-auto-sub --write-sub --sub-lang en --skip-download --sub-format vtt -o "/tmp/yt-%(id)s" "<URL>"` then read the resulting `.vtt` from `/tmp/`. Strip VTT timestamps before using.
- If `yt-dlp` isn't installed, ask the user once whether to install it with `brew install yt-dlp`. If they decline, fall back to `WebFetch` on the YouTube URL and warn that transcript extraction may be incomplete.
- Don't save the transcript into the repo. Use it in memory, then delete the temp file.

### 2. Pick the category

The categories in this repo and what goes in each:

| Category | Directory | What goes there |
|---|---|---|
| Vegetables | `vegetables/` | Vegetable-forward dishes, salads, sides that aren't mains |
| Fish | `fish/` | Fish and seafood mains |
| Meat | `meat/` | Meat mains (beef, pork, poultry, lamb, game), rubs |
| Sauce | `sauce/` | Sauces, vinaigrettes, dressings |
| Dish | `dish/` | Composed mains that don't fit cleanly into fish/meat/vegetable (dumplings, stuffed cabbage, pasta, tofu mains, savory tarts) |
| Bread | `bread/` | Yeasted breads, pizza dough, sandwich breads |
| Baking | `baking/` | Sweets, pastries, cookies, cakes, waffles, custards — **all desserts go here**, including tarts like Tarte Tatin |
| Restaurants | `resto/` | Restaurant notes, not recipes |

Edge cases:
- Savory tarts → `dish/` (see `dish/tarte_tatin_de_tomates.md`). Sweet tarts → `baking/`.
- A dessert waffle goes in `baking/`; bread-like breakfast items go in `bread/`.
- If the user passed a category hint, trust it.

### 3. Generate the filename

- snake_case, ASCII, `.md` extension.
- Strip accents (é → e, û → u, ê → e, etc.), drop punctuation, collapse spaces to underscores.
- Example: "Pâté de Campagne" → `pate_de_campagne.md`.
- If a file with that name already exists in the target category, ask the user before overwriting.

### 4. Write the recipe file

Follow the structure documented in `CLAUDE.md`:

```markdown
# <Recipe Title>

[<Source name or author>](<original URL>)

## Ingredients

| American | Metric | Ingredient |
| --- | --- | --- |
| 1 cup | 240 ml | water |
...

## Steps

- step one
- step two
...
```

Rules:
- Keep the source link near the top so it's a clear backlink.
- Ingredient table: include both American and metric columns when the source gives both. If only one is given, use a single-unit table — don't invent conversions.
- For **bread and baking** recipes, add a baker's percent column (see `bread/baguette.md` for the convention: `pct | ingredient | g`).
- Steps are bullets aimed at an experienced cook. Skip basics, but keep the specifics that matter (salt/pepper moments, temperatures, rest times, visual cues).
- **Do not** add a "Notes" section with dated entries. That section is reserved for Yvan's own cooks after he's made the recipe. Leave it off — he'll add entries later.
- Voice: terse. Don't editorialize, don't add intros, don't explain why. Match neighboring files.
- The source attribution link is the only link required. Don't fabricate additional references.

### 5. Update README.md

Add a bullet to the matching section in `README.md`:

```markdown
- [<Title>](<category>/<filename>.md)
```

Keep the section alphabetical (case-insensitive, ignoring leading accents/articles). Find the right insertion point by reading the existing bullets in that section — don't append blindly at the end.

### 6. Report back

Tell the user: the category chosen, the filename, and which README section got updated. One or two sentences.

## What not to do

- Don't commit or push. This repo's convention (from global CLAUDE.md) is local changes only unless asked.
- Don't create a new category directory. If nothing fits, ask the user.
- Don't save transcripts, raw HTML, or fetch artifacts into the repo.
- Don't add content the source didn't provide (made-up tips, conversions, substitutions).
