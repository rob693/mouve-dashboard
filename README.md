# MOUVE Project Dashboard

Live project tracking dashboard for MOUVE by Dancing with Louise.

**View live:** https://rob693.github.io/mouve-dashboard/

---

## Current Project Status (January 2026)

### Active Projects

| Project | Status | Current Phase |
|---------|--------|---------------|
| **Ladies Fitness Funnel** | In Progress | Phase 2B/2C (Lead Nurture + Calendar Sync) |
| **MOUVE Ops Library** | In Progress | Phase 4 (Content Migration) |
| **Data Integrity System** | In Progress | Phase 2 Complete, Phase 3 Planned |
| **Student Progress Reports** | In Progress | Phase 3 Ready (Phrase Bank Complete) |
| **Staff Portal (Softr)** | Planned | Phase 1 Ready, v2.0 Schema Documented |

### Waiting / Blocked

| Project | Status | Blocker |
|---------|--------|---------|
| **Children's Dance Onboarding** | Waiting | Louise email review (Phase 3) |
| **Back Office: Staff Costs** | Waiting | 5 staff rates from Louise (Phase 4) |
| **Back Office: Amex Recon** | Waiting | January statement (~5 Feb) |

### Completed

- Ops Priority Engine
- Term Renewal Engine
- Teacher Culture Programme (12 campaigns live)
- MOUVE RAG Agent
- Spring Term 2026 Welcome
- Trials Tracker System
- Dynamic Dashboard

---

## Project Highlights

### Ladies Fitness Funnel
10 active n8n workflows (LF-00 to LF-09) handling:
- TeamUp sync (375 memberships, 326 contacts)
- Self-serve trial capture
- Instant lead response (SMS + Email)
- Attendance tracking (hourly)
- Pass expiry reminders
- Trial expiry handling

### Staff Portal (Softr)
Replacing Airtable Forms with authenticated UI layer:
- ADR-010 approved
- $49/month Softr Basic
- 3 workflows documented (Timesheets, Onboarding, Progress Reports)
- Form schemas, status workflows, n8n dependencies all documented in v2.0

### Student Progress Reports
Automated end-of-term reports for children's dance:
- 282 phrases in PHRASE_BANK (132 generic + 75 Ballet + 75 Gymnastics)
- Teacher input: 5 dropdowns, <60s per child
- PDF generation + batch delivery planned

---

## Dashboard Features

- **Auto-refresh:** Every 5 minutes when open
- **Phase tracking:** Visual progress through multi-phase projects
- **Sub-phase details:** Expandable task lists
- **Blocker visibility:** Clear indication of what's waiting
- **Dependency mapping:** Data flows and system integrations

---

## Data Source

`dashboard-data.json` is the single source of truth, updated during Claude Code work sessions.

**Last updated:** 12 January 2026

---

## Related

- **MOUVE Customer Engine:** Main automation codebase
- **Airtable:** Claude Managed Contacts base
- **n8n:** https://n8n.rob-n8n.cloud
