---
name: fetch-recipe-source
description: Fetch a recipe source (YouTube video or web page) and save it verbatim into the raw/ area as a Markdown capture with the link, the video description, and the full transcript. Does not write a recipe. Use before /add-recipe, or on its own to stash a source for later.
allowed-tools: Bash, Read, Write, Glob, Grep, WebFetch
---

# fetch-recipe-source

Capture a recipe source into `raw/`. This skill **only captures**. It never writes a recipe file and never touches `README.md`. Converting a capture into a recipe is `/add-recipe`'s job.

## Why raw/ exists

The transcript is the evidence. A recipe file is a lossy summary of it. Keeping the raw capture in the repo means:

- The recipe can link to the full transcript, so the detail that didn't make the cut is one click away.
- Re-deriving or fixing a recipe later needs no re-fetch.
- Dedup: `grep -rl "<video_id>" raw/` says whether a source is already in the repo.

`raw/` is committed. It is **not** indexed in `README.md`.

## Inputs

`$ARGUMENTS` is a URL. Examples:

- `/fetch-recipe-source https://www.youtube.com/watch?v=H8owaQbV1ok`
- `/fetch-recipe-source https://youtu.be/abc123`
- `/fetch-recipe-source https://www.seriouseats.com/something`

## Steps

### 1. Dedup first

For YouTube, extract the video ID (`watch?v={id}`, `youtu.be/{id}`, `shorts/{id}`, strip query params), then:

```
grep -rl "{video_id}" raw/
```

For a web page, grep the URL. If it already exists, **stop**. Report the existing path and do not re-fetch.

### 2a. YouTube: transcript and metadata

Metadata:

```
yt-dlp --skip-download --print "%(title)s|%(channel)s|%(upload_date)s|%(duration)s" "<URL>"
```

Description (often holds the exact quantities the spoken transcript skips, so always capture it):

```
yt-dlp --skip-download --print "%(description)s" "<URL>"
```

Transcript, preferred (no download, fast):

```
python3 - <<'EOF'
from youtube_transcript_api import YouTubeTranscriptApi
api = YouTubeTranscriptApi()
t = api.fetch("{video_id}")
print(" ".join(s.text.replace("\n", " ") for s in t))
EOF
```

Fallback if that fails (rate limit, no captions API):

```
yt-dlp --write-auto-sub --write-sub --sub-lang en --skip-download --sub-format vtt -o "/tmp/yt-%(id)s" "<URL>"
```

then strip VTT timestamps and dedupe repeated caption lines.

If neither works, tell the user and stop. Don't guess a recipe from a title.

Break the transcript into paragraphs at topic shifts (roughly where the chapter list changes, or every 5-8 sentences). Keep the wording verbatim. Do not summarize, tidy grammar, or drop the chatter.

### 2b. Web page: article body

`WebFetch` the URL. Ask for the title, author/site, the full ingredient list with quantities exactly as printed, and the full method text. Keep quantities verbatim.

### 3. Write the capture

YouTube goes to `raw/youtube/`, web pages to `raw/web/`.

**Filename:** `{YYYY-MM-DD}-{slug}-{video_id}.md` for YouTube, `{YYYY-MM-DD}-{slug}.md` for web. `{YYYY-MM-DD}` is the source's publish date when known, else today. Slug is lowercase, ASCII, hyphenated, accents stripped, under 60 chars total before the ID.

YouTube template:

```markdown
---
title: "{video title}"
channel: "{channel}"
url: {original URL}
video_id: {id}
published: {YYYY-MM-DD}
duration: {M:SS}
fetched: {today YYYY-MM-DD}
type: youtube
---

# {video title}

**{channel}** · [watch]({url}) · {M:SS}

## Description

{verbatim video description, including the recipe block and chapter list if present}

## Transcript

{full verbatim transcript, in paragraphs}
```

Web template: same frontmatter minus `channel`/`video_id`/`duration`, with `type: web`, then `# {title}`, the source link, and the article body as `## Source text`.

### 4. Report

One or two lines: the path written, the source title, and whether a quantities block was found in the description. If the description had no quantities and the transcript is vague on amounts, say so loudly. That is the thing that makes the next step guess.

## What not to do

- Don't write a recipe file. Don't touch `README.md`.
- Don't summarize, trim, or paraphrase the capture. Verbatim is the point.
- Don't re-fetch a source already in `raw/`.
- Don't leave temp files in `/tmp` behind.
