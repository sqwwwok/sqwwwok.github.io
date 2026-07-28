# Agent notes for this repository

This is a Hexo blog (`sqwwwok.github.io`). Posts live under `source/_posts/`. GitHub Pages deploys on push to `main` via `.github/workflows/pages.yml`.

## Cursor Cloud specific instructions

- Single service: a static Hexo 7 blog. Standard scripts live in `package.json` (`server`, `build`, `clean`). There is no lint or automated test setup.
- Package manager is npm (CI runs `npm install`; a `yarn.lock` also exists but is not used by CI). Dependencies are refreshed by the startup update script, so you normally don't need to reinstall.
- Dev server: `npm run server` serves at `http://localhost:4000/` and hot-regenerates on file changes (e.g. `hexo new post "..."` shows up live). Run it under tmux for long-lived sessions.
- `db.json` is a generated Hexo cache that is (unusually) tracked in git; `npm run build`/`server` will modify it. Do NOT commit incidental `db.json` changes — `git checkout db.json` to discard them.
- To add content locally, use `npx hexo new post "Title"`; created files land in `source/_posts/`.

## Hacker News daily summary automation

When publishing a daily Hacker News digest:

### Do

1. Fetch the current top 10 Hacker News posts (title, source URL, score, comment count, HN discussion URL). Prefer the HN Firebase API or `news.ycombinator.com`.
2. Create **one** new Markdown post for today (Asia/Shanghai date).
3. Open a pull request that contains **only** that new post file. Do not merge the PR.

### Idempotency

- Today’s date must use timezone `Asia/Shanghai` as `YYYY-MM-DD`.
- If `source/_posts/hn-daily-YYYY-MM-DD.md` already exists, or another post for the same date already has category `【Hacknews Daily Summary】`, do nothing (no new file, no PR).
- If Memories are available: skip when `last_posted_date` equals today; after a successful PR, update `last_posted_date`.

### File path

```text
source/_posts/hn-daily-YYYY-MM-DD.md
```

### Required front matter

Match existing posts: YAML list style for `categories` / `tags`.

```yaml
---
title: Hacknews Daily Summary - YYYY-MM-DD
date: YYYY-MM-DD 09:00:00
categories:
  - 【Hacknews Daily Summary】
tags:
  - Hacker News
  - Daily Summary
---
```

Category string must be exactly `【Hacknews Daily Summary】` so posts group under that category page.

### Body structure

1. Short intro (2–4 sentences) covering the day’s themes.
2. Numbered items `1`–`10`. Each item must include:
   - Title + source link
   - Background / context
   - Main discussion directions and viewpoints
   - Explanations of proper nouns / jargon
   - HN discussion link, plus score and comment count
3. Neutral, factual tone. Do not invent details. If the source article cannot be fetched, summarize from the HN title/discussion and note that.

### PR requirements

- Branch from `main`.
- Commit only the new post file.
- PR title: `Hacknews Daily Summary - YYYY-MM-DD`
- PR body: brief note + list of the 10 titles.
- Do **not** change themes, `_config.yml`, old posts, scaffolds, or dependencies.
- Do **not** merge the PR; leave it for human review. Merge to `main` triggers site deploy.

### Quality bar

- Open a PR only when a valid new post exists with 10 items and the correct category.
- If HN is unreachable or quality is insufficient: make no repo changes and explain why.
