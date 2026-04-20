# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

Yvan's personal recipe collection ("Trois Etoiles"), published via GitHub Pages. It's a Jekyll site using the `jekyll-theme-modernist` theme (`_config.yml`). There is no build step locally — GitHub Pages renders the Markdown on push. There are no tests, no linters, and no package manager.

## Structure

- `README.md` is the site index. It's the landing page and the source of truth for what recipes are published.
- Recipes live as Markdown files in category directories: `vegetables/`, `fish/`, `meat/`, `sauce/`, `dish/`, `bread/`, `baking/`, `resto/`.
- `mama.html` is a standalone static page (not part of the recipe index).

## Adding or editing recipes

When you add a new recipe file under a category directory, also add a corresponding bullet under the matching section in `README.md`. Categories in the README are kept alphabetical within each section — match that ordering. The README is what humans navigate; an orphaned recipe file won't be discoverable.

Recipe files follow this structure:

1. **Title** — the recipe name as an `#` heading.
2. **Source links** — one or two links to where the recipe came from (YouTube videos, blogs, cookbooks). Inline, near the top.
3. **Ingredients** — as a Markdown table. Ideally two measurement columns: American (cups/tbsp/oz) and metric (grams/ml). For bread and baking, also include a baker's percent column. If only one unit system is given in the source, that's fine — don't invent conversions.
4. **Steps** — a bullet list aimed at someone who cooks regularly. Skip basics (don't say "preheat the oven" generically), but *do* call out specifics that matter: when to salt and pepper, target temperatures, rest times, visual cues.
5. **Notes from past cooks** — Yvan's own dated entries (`### 2021-02-27`) with what worked, what to change next time, variations tried. This section grows over time.

Match the style of neighboring files in the same category rather than imposing a new format.

## Voice

Recipe notes are terse, often mixed English/Dutch/French, and written for Yvan's own reference. Keep that voice. Don't expand shorthand, don't add explanatory prose, don't "clean up" tables into paragraphs.
