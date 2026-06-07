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

## Automation

### Exporter (homelab)

[`git-activity-exporter`](https://git.umi4.life/umi4life/git-activity-exporter) runs on a timer, exports counts, and pushes to this repo when `data/activity.json` changes.

### Blog vendor CI

When `data/activity.json` is pushed to `master`, GitHub Actions copies it into [`Umi4Life/umi4.life`](https://github.com/Umi4Life/umi4.life) at `static/data/activity.json` and pushes. That triggers the existing Hugo Pages deploy.

Workflow: [`.github/workflows/vendor-to-umi4life.yml`](.github/workflows/vendor-to-umi4life.yml)

### Required secret (one-time setup)

In **this repo** → Settings → Secrets and variables → Actions:

| Secret | Value |
|---|---|
| `UMI4LIFE_REPO_TOKEN` | Fine-grained PAT with **Contents: Read and write** on `Umi4Life/umi4.life` only |

Without this secret, the vendor workflow cannot commit to the blog repo.

## Operational loop

```text
homelab timer → git-activity-exporter → push git-activity (if changed)
→ vendor CI → commit umi4.life/static/data/activity.json
→ Hugo Pages deploy
```

## Manual publish (optional)

```bash
cd /path/to/git-activity
git add data/activity.json public/activity.svg
git commit -m "Update sanitized git activity artifacts"
git push
```
