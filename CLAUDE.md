# MOUVE Dashboard

> GitHub Pages dashboard for MOUVE project tracking

## RESUME HERE (16 Feb 2026 - SEO Phase 9 schema in progress)

## What This Is

Static HTML dashboard showing all MOUVE projects, KPIs, and status. Deployed via GitHub Pages.

## Key Files

| File | Purpose |
|------|---------|
| `dashboard-data.json` | **THE DATA SOURCE** - all project/task data lives here |
| `index.html` | Main dashboard UI (reads from dashboard-data.json) |
| `marketing-tasks.json` | **MARKETING SPRINT DATA** - weekly tasks, sub-projects, completed items |
| `marketing.html` | Marketing Sprint Board UI (reads from marketing-tasks.json) |

## Common Tasks

| Task | How |
|------|-----|
| Update project status | Edit `dashboard-data.json`, then `git add . && git commit -m "msg" && git push` |
| Update marketing sprints | Edit `marketing-tasks.json`, then `git add . && git commit -m "msg" && git push` |
| View main dashboard | https://rob693.github.io/mouve-dashboard/ |
| View marketing board | https://rob693.github.io/mouve-dashboard/marketing.html |
| Add new project | Add to `primary_projects` or `secondary_projects` array in JSON |
| Add task | Add to `tasks` array in JSON |
| Mark marketing task done | Move from `weeks[].tasks` to `completed` array in marketing-tasks.json, update `summary` counts |

## Marketing Sprint Board

- **URL:** https://rob693.github.io/mouve-dashboard/marketing.html
- **Data:** `marketing-tasks.json`
- **Link from main dashboard:** Quick reference bar → "Marketing Sprint Board →"
- **Structure:** 13 weekly sprints (Feb–May 2026), 6 sub-projects (infra, kids, parties, LF, events, retention)
- **Update each session:** Move completed tasks, add new discoveries, push to deploy
- **Sub-project IDs:** `infra`, `kids`, `parties`, `lf`, `events`, `retention`

### How to update marketing-tasks.json

**Mark a task done:** Remove from `weeks[N].tasks` array, add to `completed` array with `"completed": "YYYY-MM-DD"`. Update `summary.completed` count.

**Add a new task:** Add to the appropriate `weeks[N].tasks` entry with `id`, `task`, `sub_project`, `priority`, `status`.

**Update summary:** After any changes, update `summary.completed`, `summary.in_progress`, `summary.planned` counts.

## Vocabulary

- **"dashboard"** = this project (mouve-dashboard repo)
- **"dynamic dashboard"** = same thing
- **"project dashboard"** = same thing
- **"update the dashboard"** = edit dashboard-data.json and push
- **"sprint board"** / **"marketing dashboard"** = marketing.html
- **"update the sprint board"** = edit marketing-tasks.json and push

## Deployment

- **Host:** GitHub Pages
- **Repo:** https://github.com/rob693/mouve-dashboard
- **Auto-deploys:** On push to main (within ~1 min)

## Do NOT

- Edit any dashboard files in MOUVE-Customer-Engine/dashboard/ — that's an old local version
