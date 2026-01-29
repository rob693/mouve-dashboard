# Dashboard v2 — Schema & Update Instructions

**Version:** 3.0
**Updated:** 29 January 2026
**File:** `/Volumes/NVME/Claude/Code/mouve-dashboard/dashboard-data.json`
**Live (v1):** https://rob693.github.io/mouve-dashboard/
**Preview (v2):** https://rob693.github.io/mouve-dashboard/index-v2.html

---

## How This Works

`dashboard-data.json` is the single source of truth. Both `index.html` (v1) and `index-v2.html` (v2) read from the same file. The v2 layout adds new optional sections — v1 ignores them gracefully.

**After ANY change:** commit + push to GitHub. Dashboard updates in ~1 minute.

```bash
cd /Volumes/NVME/Claude/Code/mouve-dashboard
git add dashboard-data.json
git commit -m "Dashboard: [what changed]"
git push
```

---

## Complete JSON Schema

```
dashboard-data.json
├── generated_at          (ISO 8601 timestamp)
├── generated_by          ("SYS-DASHBOARD v2")
├── version               ("2.7")
├── health_summary        (project status counts)
├── tasks                 (pending + done to-do items)
├── weekly_tasks           (recurring ops tasks)
├── weekly_review          ★ NEW v2 — weekly ops/leadership summary
├── recent_decisions       ★ NEW v2 — settled decisions log
├── financial_summary      ★ NEW v2 — budget snapshot
├── system_health          ★ NEW v2 — infra status
├── primary_projects       (active projects with full detail)
├── secondary_projects     (scheduled/planned projects)
├── completed_projects     (multi_phase, simple, active_campaigns)
├── dependencies           (system integrations map)
├── data_flows             (data pipeline diagrams)
├── workflows              (n8n workflow table)
└── priority_actions       (categorised action items)
```

---

## Section-by-Section Reference

### 1. `health_summary`

Aggregate counts displayed as KPI cards.

```json
"health_summary": {
  "complete": 33,
  "in_progress": 7,
  "waiting": 1,
  "planned": 3,
  "blocked": 0
}
```

**When to update:** Whenever a project changes status (new, completed, blocked).
**How to count:** Sum all projects across primary, secondary, and completed.

---

### 2. `weekly_review` ★ NEW

Four-column leadership summary. Bridges ops work and strategic thinking.

```json
"weekly_review": {
  "week_commencing": "27 January 2026",
  "built": [
    "Staff Portal live to all 22 staff",
    "Supabase nightly sync + monitoring"
  ],
  "improved": [
    "SMS costs: ~410/month saved",
    "Venue rental accuracy: +40% correction"
  ],
  "decisions": [
    "Venue rental correction accepted (£23K→£32K)",
    "Stripe load abandoned (90% unattributable)"
  ],
  "risks": [
    "Bookkeeper response (9 questions, sent 29 Jan)",
    "AC onboarding upload (25 HTMLs pending)"
  ]
}
```

**When to update:** At session wrap-up, or weekly on Monday.
**Where to get data:**
- `built` — completed phases or deployments from this week's sessions
- `improved` — measurable improvements (cost savings, accuracy gains, fixes)
- `decisions` — choices locked this week (check CLAUDE.md RESUME HERE section)
- `risks` — items due next week that could slip (check `tasks` with upcoming due dates)

**Rules:**
- 2-4 items per column (keep it scannable)
- Use numbers where possible ("~410/month saved" not "reduced SMS costs")
- Replace entire section each week (not cumulative)

---

### 3. `recent_decisions` ★ NEW

Rolling log of settled decisions (last ~14 days). Prevents re-litigation.

```json
"recent_decisions": [
  "Accepted corrected venue rental (+£9,230)",
  "Locked Supabase as future write source",
  "Deferred Dance Studio SaaS to Q2",
  "Staff Portal: custom Next.js over Softr"
]
```

**When to update:** Whenever a significant decision is made during a session.
**Where to get data:**
- CLAUDE.md RESUME HERE section (look for "decisions", "accepted", "locked", "chose", "abandoned", "deferred")
- Implementation plan status changes
- Architecture choices

**Rules:**
- Keep 4-8 items max (trim oldest when adding new)
- Start with a verb: "Accepted", "Locked", "Deferred", "Chose", "Abandoned"
- Include the key number or trade-off in parentheses where relevant
- No timestamps needed — the rolling 14-day window is implicit
- Remove items older than ~14 days during wrap-up

---

### 4. `financial_summary` ★ NEW

Budget snapshot from the 12-Month Budget spreadsheet.

```json
"financial_summary": {
  "revenue": 418996,
  "direct_costs": 90589,
  "contribution_margin_pct": 78.4,
  "overheads": 124709,
  "marketing": 32316,
  "net_profit": 171382,
  "closing_cash_aug": 268992,
  "cash_runway_months": 5.0,
  "lowest_cash_month": "Sep (£111K)",
  "venue_corrected": true,
  "source": "12-Month Budget v2.1 (29 Jan 2026)"
}
```

**When to update:** When the budget spreadsheet changes (new actuals, corrections, re-forecasts).
**Where to get data:** `12-MONTH-BUDGET-IMPLEMENTATION-PLAN-JAN2026.md` or the Google Sheets budget.

**All values are integers (pence/pounds, no decimals) except:**
- `contribution_margin_pct` — float percentage
- `cash_runway_months` — float months
- `lowest_cash_month` — string description
- `source` — string attribution

---

### 5. `system_health` ★ NEW

Infrastructure status for the three key systems.

```json
"system_health": {
  "supabase_sync": {
    "status": "healthy",
    "last_run": "2026-01-30T02:00:00Z",
    "records": 4486
  },
  "n8n_workflows": {
    "status": "healthy",
    "active": 14,
    "failed_24h": 0
  },
  "data_quality": {
    "status": "healthy",
    "contacts": 2972,
    "orphans": 18
  }
}
```

**When to update:**
- `supabase_sync` — after verifying nightly sync (check `ops.system_health` table)
- `n8n_workflows` — after workflow changes (add/remove/fix)
- `data_quality` — after sync runs or data cleanup

**Status values:** `"healthy"` | `"warning"` | `"critical"`
- `healthy` = green dot — everything working
- `warning` = amber dot — degraded but functional
- `critical` = red dot — broken, needs attention

**Where to get data:**
- Supabase: `SELECT * FROM ops.system_health WHERE check_name='airtable_sync' ORDER BY checked_at DESC LIMIT 1;`
- n8n: `n8n MCP search_workflows` tool → count active, check execution history for failures
- Data quality: Supabase contacts count + orphan check from reconciliation

---

### 6. `tasks`

To-do items with due dates and status.

```json
{
  "title": "Task description",
  "due": "2026-02-07",
  "status": "pending",
  "context": "Which project this relates to"
}
```

**Status values:** `"pending"` | `"done"` | `"blocked"`
**When to update:** When tasks are completed, added, or due dates change.
**Rules:**
- Keep done items for ~1 week (provides completion visibility), then remove
- `due` format is always `YYYY-MM-DD`
- `context` should reference the project or phase

---

### 7. `weekly_tasks`

Recurring operational tasks (scripts, checks).

```json
{
  "name": "Prepare Sunday's Trial List",
  "frequency": "Weekly (before Sunday)",
  "command": "python3 scripts/weekly-trials-generator.py",
  "location": "MOUVE-Customer-Engine repo",
  "description": "Generate trials spreadsheet from GHL appointments",
  "sop": "docs/sops/SOP-A-020-WEEKLY-TRIALS-SPREADSHEET.md",
  "output": "~/Downloads/Trials Sunday [Date] 2026.xlsx",
  "time": "2 minutes"
}
```

**When to update:** When adding/removing recurring tasks or SOPs.

---

### 8. `primary_projects`

Active projects with full phase detail. Shown as condensed cards + timeline bars.

```json
{
  "name": "Project Name",
  "phase_current": 3,
  "phase_total": 7,
  "implementation_plan": "FILENAME.md",
  "purpose": "One-line problem statement",
  "status": "in_progress",
  "status_label": "v1.5 | Phase 3 Complete ✅",
  "blocker": "Optional — omit if none",
  "completed_phases": [
    { "name": "Phase 1: Foundation", "summary": "Brief 1-line summary" }
  ],
  "next_phase": {
    "name": "Phase 4: Engine Build",
    "detail": "Description of what this phase does",
    "status": "planned",
    "tasks": ["Task 1", "Task 2"]
  },
  "remaining_phases": [
    "Phase 5: Testing",
    "Phase 6-7: Rollout"
  ]
}
```

**Status values:** `"in_progress"` | `"planned"` | `"waiting"` | `"blocked"`
**When to update:** After completing a phase, starting a phase, or blocker changes.

**Update pattern — completing a phase:**
1. Increment `phase_current`
2. Move `next_phase` content into `completed_phases` array (name + summary)
3. Set new `next_phase` from first item in `remaining_phases`
4. Remove that item from `remaining_phases`
5. Update `status_label` with new version + phase info

**Update pattern — adding a blocker:**
1. Add `"blocker": "Description of what's blocking"`
2. Change `status` to `"blocked"` or `"waiting"`
3. Update `status_label`

**Update pattern — clearing a blocker:**
1. Remove the `blocker` field entirely (don't set to null)
2. Change `status` back to `"in_progress"` or `"planned"`

---

### 9. `secondary_projects`

Planned/scheduled projects. Minimal detail.

```json
{
  "name": "Project Name",
  "phase_current": 0,
  "phase_total": 5,
  "implementation_plan": "FILENAME.md",
  "purpose": "One-line problem statement",
  "status": "planned",
  "status_label": "v1.0 Planning Complete",
  "start_date": "February 2026",
  "next_action": "Phase 1: First step"
}
```

**Moving secondary → primary:** When work starts, move the object from `secondary_projects` to `primary_projects`, add `completed_phases: []`, `next_phase`, and `remaining_phases`.

---

### 10. `completed_projects`

Three sub-arrays:

```json
"completed_projects": {
  "multi_phase": [
    { "name": "Project Name", "result": "Production (what shipped)" }
  ],
  "simple": [
    { "name": "Task Name", "result": "What was delivered", "closed": false }
  ],
  "active_campaigns": [
    { "name": "Campaign Name", "status": "Active (4 emails)" }
  ]
}
```

**When to update:** When a project completes, move from primary/secondary into the appropriate sub-array.

---

### 11. `priority_actions`

Categorised action items.

```json
{
  "category": "BUILD",
  "action": "Short action title",
  "detail": "Longer explanation with context"
}
```

**Category values:** `"BUILD"` | `"ACTION"` | `"OPS"` | `"COMPLETE"` | `"P0"`
**When to update:** When priorities shift, actions complete, or new work is identified.

---

### 12. `dependencies`

System integration status map. Five categories:

```json
"dependencies": {
  "lead_capture": [{ "name": "Facebook Ads", "status": "active" }],
  "crm_leads": [{ "name": "GHL StudioHub", "status": "active" }],
  "automation": [{ "name": "n8n (25 workflows)", "status": "active" }],
  "data_email": [{ "name": "Airtable", "status": "active" }],
  "booking_systems": [{ "name": "TeamUp (LF)", "status": "active" }]
}
```

**Status values:** `"active"` | `"partial"` | `"blocked"` | `"reference"`

---

### 13. `data_flows`

Visual data pipeline descriptions.

```json
{
  "name": "Facebook Lead Flow",
  "steps": ["Facebook Lead", "GHL Pipeline", "n8n LF-02", "SMS + Email + Airtable"],
  "active": true
}
```

---

### 14. `workflows`

n8n workflow registry.

```json
{
  "id": "LF-01",
  "name": "Self-Serve Trial Capture v1.9",
  "trigger": "Every 5 min",
  "status": "active"
}
```

**When to update:** When workflows are added, renamed, or deactivated.

---

## Session Wrap-Up Checklist

When Claude is asked to update the dashboard (or during session wrap-up), follow this checklist:

### 1. Gather data from the session

| What changed? | Update which section? |
|---|---|
| Phase completed | `primary_projects` (phase_current, completed_phases, next_phase) |
| New project started | `primary_projects` or `secondary_projects` |
| Project completed | Move to `completed_projects` |
| Task completed | `tasks` (status → "done") |
| New task identified | `tasks` (add with due date) |
| Decision made | `recent_decisions` (append) |
| Workflow added/changed | `workflows` + `system_health.n8n_workflows` |
| Data synced/cleaned | `system_health.data_quality` |
| Budget updated | `financial_summary` |
| Blocker added/cleared | Project's `blocker` field + `status` |

### 2. Update `weekly_review` (if Monday or significant progress)

Pull from CLAUDE.md RESUME HERE:
- **built** — things shipped/deployed
- **improved** — measurable gains
- **decisions** — choices locked (also add to `recent_decisions`)
- **risks** — things due soon that could slip

### 3. Update metadata

```json
"generated_at": "2026-01-29T21:30:00Z",  ← current timestamp
"version": "2.7",                          ← increment
```

### 4. Update `health_summary` counts

Recount across all project arrays:
- `complete` = completed_projects.multi_phase.length + completed_projects.simple.length
- `in_progress` = projects with status "in_progress"
- `waiting` = projects with status "waiting"
- `planned` = projects with status "planned"
- `blocked` = projects with status "blocked"

### 5. Commit + push

```bash
cd /Volumes/NVME/Claude/Code/mouve-dashboard
git add dashboard-data.json
git commit -m "Dashboard v[X.Y]: [summary of changes]"
git push
```

---

## Do / Don't

### DO
- Update dashboard after every phase change
- Keep `purpose` to 1 line max
- Use numbers in `improved` and `financial_summary`
- Start `recent_decisions` items with a verb
- Remove done tasks after ~1 week
- Trim `recent_decisions` to last ~14 days

### DON'T
- Don't set blocker to null — remove the field entirely
- Don't expand completed phases beyond 1-line summaries
- Don't add projects without implementation plans
- Don't leave stale `weekly_review` data (update or remove)
- Don't put timestamps in `recent_decisions`
- Don't exceed 8 items in `recent_decisions`
