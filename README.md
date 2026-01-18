# MOUVE Project Dashboard v2.0

Live project tracking dashboard for MOUVE by Dancing with Louise.

**View live:** https://rob693.github.io/mouve-dashboard/

---

## Dashboard v2.0 (17 Jan 2026)

### Structure

```
📋 Weekly Tasks (top priority - most frequent reference)
    ↓
🎯 Primary Projects (4 urgent/active projects)
    ↓
📌 Secondary Projects (4 scheduled/planned projects)
    ↓
✅ Completed Projects
    ↓
System Dependencies, Data Flows, Workflows, Priority Actions
```

### Key Features

- **52% less visual clutter** vs v1.0 (collapsed completed phases)
- **PURPOSE statements** - One-line problem statement for every project
- **▶ NEXT phase prominence** - Full task detail for current/upcoming phase
- **Phase progress** - Clear (Phase 2/7) indicators
- **Auto-refresh** - Every 5 minutes
- **Health checks** - Live system status for production pipelines

---

## Current Projects (January 2026)

### 🎯 Primary Projects (4)

| Project | Phase | Status | Purpose |
|---------|-------|--------|---------|
| **Ladies Fitness Funnel** | 3/7 | ✅ EXCELLENT | Automate lead-to-member journey (trials, attendance, retention) |
| **Children's Dance Onboarding** | 2/7 | ✅ v6.0 | Automated 10-email onboarding sequence |
| **Back Office: Staff Costs** | 4.5/7 | 📋 Ready | Automate monthly staff statements (save 3-4 hours/month) |
| **Student Progress Reports** | 2/5 | ✅ 282 phrases | Phrase-bank reports (<60s per student, zero admin editing) |

### 📌 Secondary Projects (4)

| Project | Phase | Target | Purpose |
|---------|-------|--------|---------|
| **Weekly Auto-Blog System** | 0/6 | 31 Jan 2026 | Auto-generate camp blogs with GPT-4o (save 8-10h/month) |
| **Amex Recon vNext** | 0/5 | Feb 2026 | Automate receipt routing (reduce 3 hours → 30 minutes) |
| **Staff Portal (Softr)** | 0/5 | TBD | Authenticated portal for staff (£49/month) |
| **Operational Dashboard** | 0/3 | TBD | Business metrics for Louise/Georgia |

---

## System Health (16 Jan 2026)

### Ladies Fitness Pipeline ✅ EXCELLENT
- **12 active workflows** (LF-00 to LF-11)
- **Last sync:** < 7 hours ago
- **Data flow:** Operational (1 active trial tracked correctly)
- **Webhooks:** Both endpoints HTTP 200 OK
- **TeamUp:** 326 customers, 375 memberships syncing daily

### Key Workflows
- **LF-00:** TeamUp → Airtable Sync (daily 6am)
- **LF-01:** Self-Serve Trial Capture (every 5 min)
- **LF-02:** Instant Lead Response (webhook)
- **LF-04:** Attendance Tracker (hourly)
- **LF-10:** Calendar Sync (daily 5am)
- **LF-11:** Trial Event Creator (real-time)

---

## Recent Completions

### Multi-Phase Projects
- ✅ Data Integrity System (DATA_HEALTH + SYS-01 reconciliation)
- ✅ MOUVE Ops Library (4 SOPs + 1 Playbook live)
- ✅ Ladies Fitness Calendar Sync (LF-10 + LF-11 deployed)
- ✅ Ops Priority Engine
- ✅ Term Renewal Engine
- ✅ Teacher Culture Programme (12 campaigns live)

### Active Campaigns
- 🔵 Lapsed Parent Win-Back (4 emails)
- 🔵 Ladies Fitness Win-Back (4 campaigns)
- 🔵 Teacher Culture Tuesdays (Week 2 of 12)
- 🔵 February Camp 2026 (18 enrolled, 16-20 Feb)
- 🔵 February WICKED Camp Email Campaign (7 emails, 1,726 recipients)

---

## Data Source

**File:** `dashboard-data.json`
**Updated:** During Claude Code work sessions
**Format:** Single source of truth with auto-update protocol

### Structure Documentation

**See:** `DASHBOARD-STRUCTURE-V2.md` for complete documentation of:
- JSON schema (primary_projects, secondary_projects)
- Rendering logic
- Update protocol
- Design principles
- Common patterns

---

## Technical Details

### Dependencies

| Category | Systems | Status |
|----------|---------|--------|
| **Lead Capture** | Facebook Ads, Website Forms, TeamUp Trials | ✅ Active |
| **CRM / Leads** | GHL StudioHub, GHL Workflows | ✅ Active (partial) |
| **Automation** | n8n (25 workflows), Webhooks | ✅ Active |
| **Data / Email** | Airtable, ActiveCampaign, M365 Email | ✅ Active |
| **Booking Systems** | TeamUp (LF), DSP (Dance) | ⚠️ TeamUp active, DSP blocked |

### Data Flows

**Active Flows:**
1. Facebook Lead → GHL Pipeline → n8n LF-02 → SMS + Email + Airtable
2. TeamUp Trial → n8n LF-01 → GHL Contact → Welcome SMS + Email
3. TeamUp Attendance → n8n LF-04 (hourly) → Airtable LF_MEMBERSHIPS
4. TeamUp Events → n8n LF-10 (daily 5am) → Google Calendar → LF Staff
5. TeamUp Trial Booking → n8n LF-11 (real-time) → Google Calendar Event → Staff Notification

---

## Related Projects

- **MOUVE Customer Engine:** Main automation codebase
- **Airtable:** Claude Managed Contacts base
- **n8n:** https://n8n.rob-n8n.cloud
- **ActiveCampaign:** Marketing automation & CRM
- **GoHighLevel:** StudioHub lead management

---

## Version History

### v2.0 (17 Jan 2026)
- Split projects into Primary/Secondary categories
- Added Student Progress Reports
- Moved Weekly Tasks to top
- Collapsed completed phases (summary only)
- Added PURPOSE statements to all projects
- Phase count in titles (Phase X/Y)
- 52% visual clutter reduction

### v1.0 (12 Jan 2026)
- Initial dynamic dashboard
- Auto-refresh every 5 minutes
- Phase tracking with sub-phases
- Dependency mapping
- Data flow visualization

---

## Maintenance

**Update Protocol:** See `DASHBOARD-STRUCTURE-V2.md`

**Quick Update:**
```bash
cd /Volumes/NVME/Claude/Code/mouve-dashboard
# Edit dashboard-data.json
git add dashboard-data.json
git commit -m "Update [Project]: [Change]"
git push
# Live in ~1 minute via GitHub Pages
```

---

**Last updated:** 17 January 2026
**Version:** 2.0
**Maintained by:** Claude Code (SYS-DASHBOARD v2)
