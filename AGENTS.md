# Agent notes for this repository

This is a Hexo blog (`sqwwwok.github.io`). Posts live under `source/_posts/`. GitHub Pages deploys on push to `main` via `.github/workflows/pages.yml`.

## Hacker News daily summary automation

When publishing a daily Hacker News digest:

### Do

1. Fetch the current top 10 Hacker News posts (title, source URL, score, comment count, HN discussion URL). Prefer the HN Firebase API or `news.ycombinator.com`.
2. Create **one** new Markdown post for today (Asia/Shanghai date).
3. Open a pull request that contains **only** that new post file.

### Merge behavior

Cursor Automations do **not** merge PRs. This repo’s GitHub Action `.github/workflows/auto-merge-hn-daily.yml` auto-merges matching HN daily PRs into `main` (which then triggers Pages deploy).

The agent should:

- Open the PR (draft is fine; the Action marks it ready)
- **Not** attempt to merge, push to `main`, or change the workflow files
- Stop after the PR exists

### Idempotency

- Today’s date must use timezone `Asia/Shanghai` as `YYYY-MM-DD`.
- If `source/_posts/hn-daily-YYYY-MM-DD.md` already exists, or another post for the same date already has category `【Hacknews Daily Summary】` / `Hacknews Daily Summary`, do nothing (no new file, no PR).
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

Category string should be exactly `【Hacknews Daily Summary】` so posts group under that category page.

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

- Branch from `main` (Cursor’s default branch workflow is fine).
- Prefer a head branch name starting with `cursor/hacker-news` so auto-merge can recognize it.
- Commit only the new post file.
- PR title: `Hacknews Daily Summary - YYYY-MM-DD`
- PR body: brief note + list of the 10 titles.
- Do **not** change themes, `_config.yml`, old posts, scaffolds, workflows, or dependencies.

### Quality bar

- Open a PR only when a valid new post exists with 10 items and the correct category.
- If HN is unreachable or quality is insufficient: make no repo changes and explain why.
