---
description: >
  This skill should be used when the user wants to monitor grant opportunities,
  funding notices, NOFOs, application deadlines, eligibility changes, match
  requirements, or municipal priority fit. Triggers include grant radar,
  grant watcher, funding monitor, watch grants, NOFO alert, application deadline,
  or find grants for municipal priorities.
---

# Grant Radar

Monitor funding opportunities and decide which ones deserve staff time before deadlines sneak up.

## Trigger

User invokes `/municipal-governance:grant-radar`

## Purpose

This watcher is a recurring opportunity filter. It identifies new or approaching grants, scores local fit, and routes promising opportunities into staff review or deeper analysis. It does not submit applications or commit the municipality to pursue funding.

## Inputs

- Priority areas, projects, departments, or capital needs to watch
- Funding sources: federal, state, county, MPO/COG, foundations, municipal league, agency newsletters, uploaded NOFOs
- Watch window and deadline horizon
- Optional prior watcher log from `watchers/grant-radar.md`
- Local match capacity or budget constraints, if known

## Workflow

### 0. Scope the Radar

Ask only what is needed:

1. **"What priorities or projects should I watch for?"** - infrastructure, public safety, housing, sustainability, digital services, parks, economic development, or all configured priorities.
2. **"What funding sources should I include?"** - state, federal, county, MPO/COG, foundation, or uploaded opportunities.
3. **"What should count as worth an alert?"** - deadline within 60 days, strong priority fit, low match, high award amount, recurring program, or council action needed.

If the user asks for recurring monitoring, produce a self-contained automation prompt and recommended cadence. Do not subscribe to newsletters, submit forms, contact agencies, or update external trackers without explicit confirmation.

### 1. Load Municipal Context

Read `municipal.local.md` for:
- Policy priorities
- Population and eligibility-relevant characteristics
- Counties, MPO, COG, and regional partners
- Fiscal year and budget context
- Grant match capacity if listed
- Standing plans and capital improvement references

If available, read `standing-documents.md` or project `documents/plans/` summaries for strategic plan, comprehensive plan, CIP, climate plan, housing plan, or technology plan alignment.

### 2. Load State Reference and Check Freshness

Read `state-references/{STATE}.md` when assessing state grant authority, state agency context, procurement/compliance obligations, intergovernmental authority, or public-finance requirements.

Check `last_verified`, `freshness_window_days`, and `verified_against`. If stale, missing, or lacking metadata, use it as background only and mark state-law or compliance conclusions for verification.

### 3. Retrieve Funding Sources

Use official sources where possible:
- Grants.gov, SAM.gov, Federal Register, agency NOFO pages
- State agency grant portals
- State municipal league alerts
- County, MPO, COG, or regional planning announcements
- Uploaded NOFOs, newsletters, or staff memos
- `document-management` when authenticated

If current web/source access is unavailable, work from uploaded materials and mark availability/deadline claims as unverified. Grant deadlines and eligibility rules must come from current source documents before the user relies on them.

### 4. Compare Against Prior Memory

Look for prior context in:
1. `watchers/grant-radar.md`
2. Prior `intergovernmental-scan` or grant research outputs
3. `research/`
4. `documents/plans/` and CIP notes

Detect:
- New opportunity matching a priority
- Deadline approaching
- Eligibility, match, award size, or NOFO changed
- Previously tracked opportunity opened, closed, extended, or awarded
- Council action, resolution, match commitment, or partnership letter needed

If no prior memory exists, produce a baseline opportunity table.

### 5. Score Opportunities

Use the `intergovernmental-relations` grant go/no-go rubric:
- Strategic alignment
- Staff capacity
- Match affordability
- Compliance burden
- Sustainability
- Competitiveness
- Community impact

Classify:

🔴 **Act Now**
- Deadline within 30 days and strong fit
- Council resolution, match commitment, public hearing, or partner approval needed soon
- Large award or strategic priority with narrow window

🟡 **Evaluate**
- Deadline within 31-90 days
- Good fit but match, staffing, or eligibility needs verification
- Recurring program worth preparing for next cycle

🟢 **Track**
- Low fit, long deadline, or information-only opportunity
- Closed opportunity worth watching for the next cycle

For decision-relevant claims, include confidence and provenance inline. Tag deadlines, eligibility, award amounts, match requirements, scoring, local fit, council-action needs, and compliance burdens with sources such as `NOFO p.3`, `agency grant page`, `Grants.gov`, `state agency`, `state-reference/IL last_verified 2026-03-01`, `strategic plan`, or `model inference`.

### 6. Route Follow-Up

Recommend:
- `intergovernmental-scan` for broader funding/mandate context
- `policy-research` for program design or evidence base
- `budget-review` for match and sustainability
- `council-communication` for resolution or staff memo drafting
- `agenda-watcher` if a council authorization must appear on an agenda

Draft staff questions, partner outreach, council memo bullets, or a go/no-go summary if requested. Do not send outreach or submit applications without explicit confirmation.

### 7. Update Local Radar Memory

If the user wants persistence, propose an update to `watchers/grant-radar.md`:
- Opportunity name and source URL
- Deadline and next milestone
- Fit score and confidence
- Match/compliance notes
- Follow-up owner/status if known
- Next check date

Show the update before writing. Keep the record factual and avoid private personnel, confidential negotiation, privileged legal, or campaign material.

## Output Format

```markdown
# Grant Radar Report

**Municipality**: [Name]
**Run Date**: [Date/time]
**Watch Scope**: [Priorities/projects/sources]
**Deadline Horizon**: [Dates]

## Source and Confidence Notes
- Funding sources checked: [official portals / municipal league / uploaded NOFOs / document-management]
- Standing plan sources: [strategic plan / CIP / none]
- State reference: [state-reference/XX last_verified YYYY-MM-DD / stale / unavailable]
- Prior memory source: [watchers/grant-radar.md / prior research / none]
- Claims requiring verification: [short list]

---

## Opportunities

### 🔴 Act Now
| Opportunity | Deadline | Fit | Why It Matters | Next Step |
|-------------|----------|-----|----------------|-----------|
| [Grant] | [Date] *(Confidence: [High/Medium/Low]; provenance: [source])* | [Score/High] | [Reason] | [Action] |

### 🟡 Evaluate
| Opportunity | Deadline | Fit Question | Suggested Action |
|-------------|----------|--------------|------------------|
| [Grant] | [Date] | [Eligibility/match/staffing] | [Action] |

### 🟢 Track
- [Opportunity]: [brief status or next cycle]

---

## Go/No-Go Snapshot

### [Opportunity Name]
| Factor | Score | Notes |
|--------|-------|-------|
| Strategic alignment | [1-5] | [Notes] |
| Staff capacity | [1-5] | [Notes] |
| Match affordability | [1-5] | [Notes] |
| Compliance burden | [1-5] | [Notes] |
| Sustainability | [1-5] | [Notes] |
| Competitiveness | [1-5] | [Notes] |
| Community impact | [1-5] | [Notes] |
| **Total** | **[X/35]** | **[Go/Conditional/No-Go]** |

## Upcoming Deadlines
| Date | Opportunity | Action Needed |
|------|-------------|---------------|
| [Date] | [Grant] | [Action] |

## Recommended Follow-Up
- [ ] [Verify eligibility with agency/staff]
- [ ] [Confirm match source with finance]
- [ ] [Prepare council authorization or partner letter]
- [ ] [Watch again on date]

## Analysis Boundaries

*This radar report is a single-instance funding screen, not a grant application review.*

**Items requiring verification before pursuing:**
- [Eligibility or deadline]
- [Match requirement or allowable cost]
- [Council authorization, procurement, or compliance requirement]
- [Staff capacity or competitiveness estimate]
```

## Recommended Automation Pattern

Run this radar:
- Monthly for general opportunities
- Weekly during known state/federal grant cycles
- Daily in the final two weeks before a priority deadline
- After budget/CIP updates or council priority changes

Automation prompt:

```text
Run /municipal-governance:grant-radar for [municipality]. Watch [priority areas/sources] for new, changed, or approaching grant opportunities within [deadline horizon]. Compare against prior radar memory if present. Score opportunities using the grant go/no-go rubric and flag council action, match, eligibility, and compliance needs. Include confidence/provenance and do not submit, subscribe, contact, sync, or write files without confirmation.
```

## Skills Referenced

- `intergovernmental-relations` - grant lifecycle, go/no-go scoring, regional partners
- `public-finance` - match, sustainability, and fiscal impact
- `policy-evaluation` - strategic fit and program logic
- `council-communication` - council memo, resolution, and partner communication drafting
- `intergovernmental-scan` - broader state/federal funding context

## Related Skills

- `statehouse-monitor` - discovers state/federal funding changes
- `budget-review` - evaluates match and recurring cost exposure
- `agenda-watcher` - catches authorization items on upcoming agendas
- `skill-qa` - checks provenance, freshness, and external-action gates

## Notes

- Grant fit is a triage signal, not a commitment to apply.
- Do not bury match or post-award compliance burden; these often determine whether "free money" is actually useful.
- Distinguish open NOFOs from forecasted, rumored, or prior-year programs.
- If a grant requires lobbying, partnership commitments, or council action, draft next steps but require confirmation before outreach.
