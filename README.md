# git-activity

Public sanitized git activity artifacts for [umi4.life](https://umi4.life).

This repository contains **date-level commit counts only**. It must never include private repository names, commit messages, branch names, file paths, issue titles, or tokens.

## Files

| Path | Purpose |
|---|---|
| `data/activity.json` | Sanitized daily activity (`date`, `github`, `private`, `total`) |
| `public/activity.svg` | Optional static preview for README or embeds |

## Data contract

```json
{
  "date": "2026-06-01",
  "github": 2,
  "private": 5,
  "total": 7
}
```

- `github` — public GitHub activity count for that day
- `private` — sanitized self-hosted Git activity count for that day
- `total` — sum of `github` and `private`

## Privacy boundary

Allowed in committed files:

- date strings
- non-negative integer counts

Not allowed:

- repository names or URLs
- commit messages
- branch names
- file paths
- tokens or credentials

## Blog consumption (later)

The Hugo blog at `Umi4Life/umi4.life` vendors `data/activity.json` into `static/data/activity.json` during deploy. The blog renderer does not fetch private APIs directly.

## Manual remote setup

Create an empty public repository on GitHub first: `Umi4Life/git-activity`

```bash
cd D:/Documents/Projects/git-activity
git init
git add .
git commit -m "Initial sanitized git activity artifacts"
git branch -M main
git remote add origin https://github.com/Umi4Life/git-activity.git
git push -u origin main
```

## Current status

Initial `data/activity.json` uses sample counts for scaffolding. Replace with exporter-generated output from `git-activity-exporter` when live export is implemented.
