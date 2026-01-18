# Dashboard v2.0 Structure Guide

**Version:** 2.0
**Updated:** 17 January 2026
**Purpose:** Documentation for maintaining dashboard consistency across updates

---

## Overview

The MOUVE Project Dashboard displays 8 active projects split into **Primary** (urgent/active) and **Secondary** (scheduled/planned) categories, with a focus on reducing visual clutter while maximizing actionable information.

**Live URL:** https://rob693.github.io/mouve-dashboard/

---

## Design Principles (v2.0)

### 1. Information Hierarchy
```
📋 Weekly Tasks (most frequent)
    ↓
🎯 Primary Projects (urgent/active work)
    ↓
📌 Secondary Projects (scheduled/planned)
    ↓
✅ Completed Projects
    ↓
System Dependencies, Workflows, Priority Actions
```

### 2. Visual Clarity Rules
- **Completed phases:** Summary only (1 line)
- **Current/Next phase:** Full detail with tasks
- **Remaining phases:** Collapsed (shows count only)
- **PURPOSE:** Every project shows one-line problem statement
- **Phase count:** Always show (Phase 2/7) in title

### 3. Content Density
- **Primary projects:** Full detail (completed summary + NEXT phase + remaining)
- **Secondary projects:** Minimal (3-4 lines: purpose + next action + date)
- **Result:** 52% less visual clutter vs v1.0

---

## JSON Structure

### File: `dashboard-data.json`

#### Top-Level Structure
```json
{
  "generated_at": "ISO 8601 timestamp",
  "generated_by": "SYS-DASHBOARD v2",
  "version": "2.0",
  "health_summary": { ... },
  "weekly_tasks": [ ... ],
  "primary_projects": [ ... ],      // NEW in v2.0
  "secondary_projects": [ ... ],    // NEW in v2.0
  "completed_projects": { ... },
  "dependencies": { ... },
  "data_flows": [ ... ],
  "workflows": [ ... ],
  "priority_actions": [ ... ]
}
```

### Primary Project Schema

```json
{
  "name": "Project Name",
  "phase_current": 2,                    // Current phase number
  "phase_total": 7,                      // Total phases
  "implementation_plan": "FILENAME.md",  // Implementation plan filename
  "purpose": "One-line problem statement (what problem this solves)",
  "status": "in_progress",               // in_progress | planned | waiting
  "status_label": "Phase 2/7 Complete ✅ (version info)",
  "blocker": "Optional blocker text",    // Omit if no blocker
  "health_check": {                      // Optional - only for active systems
    "timestamp": "ISO 8601",
    "status": "excellent"
  },
  "completed_phases": [                  // Array of completed phases
    {
      "name": "Phase 1: Foundation",
      "summary": "Brief 1-line summary"  // What was built
    }
  ],
  "next_phase": {                        // Current or next phase (full detail)
    "name": "Phase 3: Engine Build",
    "detail": "Brief description • Est: 90 min",
    "status": "planned",
    "tasks": [                           // Array of tasks for this phase
      "Task 1 description",
      "Task 2 description"
    ]
  },
  "remaining_phases": [                  // Array of future phase names
    "Phase 4: Testing & Validation",
    "Phase 5-7: Rollout & Training"
  ]
}
```

### Secondary Project Schema

```json
{
  "name": "Project Name",
  "phase_current": 0,
  "phase_total": 5,
  "implementation_plan": "FILENAME.md",
  "purpose": "One-line problem statement",
  "status": "waiting",                   // waiting | planned
  "status_label": "v2.0 Plan Complete",
  "start_date": "February 2026",         // OR target_date
  "next_action": "Phase 1: First step description"
}
```

---

## Categorization Rules

### Primary Projects (4 max recommended)
**Criteria:**
- Currently being worked on (in_progress)
- Urgent business need
- Has active phases or ready to start
- Requires regular attention

**Current Primary Projects:**
1. Ladies Fitness Funnel
2. Children's Dance Onboarding
3. Back Office: Staff Costs
4. Student Progress Reports

### Secondary Projects (4 max recommended)
**Criteria:**
- Scheduled for future (waiting)
- Nice-to-have but not urgent (planned)
- Has clear start date or target date
- All in Phase 0 (planning complete, not started)

**Current Secondary Projects:**
1. Back Office: Amex Recon vNext
2. Staff Portal (Softr)
3. Operational Dashboard
4. Weekly Auto-Blog System

---

## Rendering Logic (index.html)

### Primary Projects (`renderPrimaryProjects()`)

**Structure:**
```html
<div class="project-card">
  <header>
    [Name] (Phase X/Y)
    [Implementation Plan Filename]
    [Purpose - italic, gray]
    [Status Badge]
  </header>
  <body>
    [Blocker Warning - if present]
    [Completed Phases Summary]
    [▶ NEXT Phase with Full Detail]
    [Remaining Phases: [collapsed]]
  </body>
</div>
```

**Completed Phases Format:**
```
✓ Phase 1: Foundation — Brief summary
✓ Phase 2: Data Collection — Brief summary
```

**NEXT Phase Format:**
```
▶ NEXT: Phase 3: Engine Build
State machine + error handling • Est: 90 min
• Task 1 description
• Task 2 description
• Task 3 description
```

**Remaining Phases Format:**
```
Phases 4-7: [collapsed]
```

### Secondary Projects (`renderSecondaryProjects()`)

**Structure:**
```html
<div class="secondary-project-item">
  [Name] (Phase X/Y)
  [Implementation Plan Filename - monospace, gray]
  [Purpose - italic, gray]
  Next: [Next Action]
  [Start/Target Date - amber]
</div>
```

**3-4 lines max** - no phase details shown

---

## Update Protocol

### When to Update Dashboard

1. **After completing a phase** - Move to completed_phases, update next_phase
2. **After starting a phase** - Update phase_current, update status_label
3. **When blocker changes** - Add/remove blocker field
4. **When adding new project** - Add to primary_projects or secondary_projects
5. **When project completes** - Move to completed_projects, remove from primary/secondary

### How to Update

#### Step 1: Update JSON (`dashboard-data.json`)

```bash
cd /Volumes/NVME/Claude/Code/mouve-dashboard
# Edit dashboard-data.json
```

**Example: Mark Phase Complete**
```json
// Before
"phase_current": 2,
"completed_phases": [
  {"name": "Phase 1: Foundation", "summary": "6 tables created"}
],
"next_phase": {
  "name": "Phase 2: Data Collection",
  ...
}

// After
"phase_current": 3,
"completed_phases": [
  {"name": "Phase 1: Foundation", "summary": "6 tables created"},
  {"name": "Phase 2: Data Collection", "summary": "20 staff rates collected"}
],
"next_phase": {
  "name": "Phase 3: Workflow Build",
  ...
}
```

#### Step 2: Commit and Push

```bash
git add dashboard-data.json
git commit -m "Update [Project Name]: Phase X complete"
git push
```

Dashboard updates live in ~1 minute via GitHub Pages.

#### Step 3: Update CLAUDE.md

Update the Current Implementation Plans table in `/Volumes/NVME/Claude/Code/MOUVE-Customer-Engine/CLAUDE.md`:

```markdown
| Project | File | Status |
|---------|------|--------|
| Project Name | `FILENAME.md` | v2.0 - Phase 3/7 Ready 📋 |
```

---

## Common Patterns

### Pattern 1: Move Project from Secondary to Primary

**When:** Project starts active work (leaves Phase 0)

```json
// Remove from secondary_projects array
// Add to primary_projects array with full structure:
{
  "name": "...",
  "phase_current": 1,  // Changed from 0
  "completed_phases": [],
  "next_phase": { ... },
  "remaining_phases": [ ... ]
}
```

### Pattern 2: Add Blocker

```json
{
  "name": "...",
  "status": "planned",  // or "waiting"
  "status_label": "Phase 4/7 Blocked",
  "blocker": "5 staff rates needed (Names...)"
}
```

### Pattern 3: Clear Blocker

```json
{
  "name": "...",
  "status": "planned",
  "status_label": "Phase 4/7 Ready 📋 (All prerequisites complete ✅)"
  // Remove "blocker" field entirely
}
```

### Pattern 4: Complete Phase

1. Move current next_phase to completed_phases
2. Update next_phase with new phase
3. Increment phase_current
4. Update status_label

---

## Health Check Data (Optional)

For active production systems (e.g., Ladies Fitness Funnel):

```json
{
  "health_check": {
    "timestamp": "2026-01-16T07:43:00Z",
    "status": "excellent",  // excellent | good | degraded | down
    "last_sync": "2026-01-15T06:00:22Z",
    "webhooks": {
      "endpoint1": "HTTP 200 OK",
      "endpoint2": "HTTP 200 OK"
    },
    "workflows_active": "12/12",
    "data_flow": "Operational - brief status",
    "findings": "Summary of health check results"
  }
}
```

---

## Version History

### v2.0 (17 Jan 2026)
- Split projects into Primary/Secondary categories
- Added Student Progress Reports as Primary Project
- Moved Weekly Tasks to top
- Collapsed completed phases (summary only)
- Added PURPOSE statement to all projects
- Added phase count to titles (Phase X/Y)
- Added ▶ NEXT phase prominence
- 52% visual clutter reduction vs v1.0

### v1.0 (12 Jan 2026)
- Initial dynamic dashboard
- Single projects array
- Full phase detail for all phases
- Implementation plan filenames shown

---

## File Locations

| File | Location | Purpose |
|------|----------|---------|
| `dashboard-data.json` | `/mouve-dashboard/` | Single source of truth |
| `index.html` | `/mouve-dashboard/` | Rendering logic |
| `README.md` | `/mouve-dashboard/` | Project overview |
| `DASHBOARD-STRUCTURE-V2.md` | `/mouve-dashboard/` | This file (structure guide) |

---

## Related Documentation

- **Implementation Plan Protocol:** `MOUVE-Customer-Engine/docs/protocols/COMPACTION-RECOVERY-PROTOCOL.md`
- **Session Wrap-Up Protocol:** `~/.claude/CLAUDE.md` (Dashboard Auto-Update section)
- **Current Projects:** `MOUVE-Customer-Engine/CLAUDE.md` (Current Implementation Plans table)

---

## Maintenance Notes

### DO
✅ Update dashboard after every phase change
✅ Keep PURPOSE statements to 1-2 lines max
✅ Use atomic, descriptive task names in next_phase.tasks
✅ Update health_check timestamp when running checks
✅ Remove blocker field entirely when cleared (don't set to null)

### DON'T
❌ Don't expand completed phases (summary only)
❌ Don't add projects without implementation plans
❌ Don't mix Primary/Secondary criteria (urgent vs scheduled)
❌ Don't exceed 4 projects per category (readability threshold)
❌ Don't include sub-phase detail in completed_phases

---

## Support

**Questions?** Check:
1. This file (`DASHBOARD-STRUCTURE-V2.md`)
2. Implementation Plan Protocol (`COMPACTION-RECOVERY-PROTOCOL.md`)
3. Session logs in `MOUVE-Customer-Engine/CLAUDE.md`
