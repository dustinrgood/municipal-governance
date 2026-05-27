---
description: >
  This skill should be used when the user wants recurring monitoring of state
  legislation, agency rules, preemption threats, mandates, municipal-league
  alerts, or federal developments that may affect the municipality. Triggers
  include statehouse monitor, watch state bills, legislative monitor, preemption
  alert, mandate watcher, session tracker, or state law monitoring.
---

# Statehouse Monitor

Monitor state and federal developments that could affect local authority, compliance duties, funding, or municipal operations.

## Trigger

User invokes `/municipal-governance:statehouse-monitor`

## Purpose

This is an automation-oriented companion to `intergovernmental-scan`. It is optimized for recurring monitoring: detect what changed since the last run, classify urgency, and route significant findings into deeper analysis.

## Inputs

- Jurisdiction and state from `municipal.local.md`
- Topics or bill lists to watch
- Scan period or recurrence window
- Official legislative, agency, municipal league, or uploaded source links
- Optional prior watcher log from `watchers/statehouse-monitor.md`

## Workflow

### 0. Scope the Monitor

Ask only what is needed:

1. **"What should I watch?"** - full session, named bills, preemption topics, grants/funding, agency rules, or federal actions.
2. **"What should trigger an alert?"** - bill movement, enacted law, effective date approaching, municipal mandate, preemption risk, comment deadline, or advocacy window.
3. **"How often should this run?"** - weekly, daily during session, or topic-specific cadence.

If the user already names the scope, proceed. If the user asks for a recurring setup, produce a self-contained automation prompt and recommended cadence, but do not subscribe, send, or post externally without explicit confirmation.

### 1. Load Municipal Context

Read `municipal.local.md` for:
- State
- Home rule status
- Counties and regional context
- Policy priorities
- Fiscal thresholds
- Legislative contacts or municipal league references, if listed

### 2. Load State Reference and Check Freshness

Read `state-references/{STATE}.md` before giving current-law, home-rule, preemption, mandate, grant-authority, procurement, OMA, FOIA, or ethics guidance.

Check `last_verified`, `freshness_window_days`, and `verified_against`. If the reference is stale, missing, or lacks metadata, use it only as background and verify current status against official legislature, agency, or municipal league sources before treating it as current.

### 3. Retrieve Current Sources

Prioritize official or high-quality sources:
- State legislature bill search and bill text
- State agency rulemaking notices
- Governor's office actions
- State municipal league alerts
- Federal Register, Congress.gov, federal agency notices
- Uploaded bill lists, league emails, or staff memos

If web or connector access is unavailable, work from uploaded sources and clearly mark current status as unverified. Legislative status changes quickly; never rely on memory for current bill status.

### 4. Compare Against Prior Memory

Look for prior context in:
1. `watchers/statehouse-monitor.md`
2. Prior `intergovernmental-scan` reports
3. `research/`
4. Current conversation context

Detect:
- New bill introduction
- Committee assignment, hearing, amendment, passage, signature, veto, or effective date change
- New municipal league position
- Agency rulemaking notice, comment deadline, or guidance
- Federal grant, mandate, or compliance update
- Previously watched bill becoming inactive, dead, enacted, or superseded

If no prior memory exists, produce a baseline watchlist and status table.

### 5. Classify Findings

🔴 **Alert Now**
- Direct mandate or compliance deadline
- Preemption or restriction of local authority
- Effective date, hearing, or comment deadline within 14 days
- Fiscal exposure, grant match, or implementation cost likely above local threshold
- Recommended advocacy position or council action

🟡 **Monitor**
- Bill movement with plausible municipal impact
- Agency proposal with uncertain applicability
- Municipal league alert requiring staff review
- Grant/funding opportunity needing eligibility screening

🟢 **Track Only**
- Informational update
- Low-impact bill movement
- Topic remains inactive

For decision-relevant claims, include confidence and provenance inline. Tag bill status, amendment summaries, effective dates, mandate/preemption conclusions, grant deadlines, fiscal estimates, and advocacy recommendations with sources such as `state legislature`, `bill text`, `municipal league`, `agency notice`, `Federal Register`, `state-reference/IL last_verified 2026-03-01`, or `model inference`.

### 6. Route Follow-Up

Recommend:
- `intergovernmental-scan` for a broader report
- `policy-research` for decision-focused alternatives
- `analyze-ordinance` if local code must change
- `budget-review` for unfunded mandates or grant match
- `grant-radar` for funding opportunities

If a position letter, testimony outline, or council update is requested, draft it only. Do not send, post, or file it without explicit confirmation.

### 7. Update Local Monitor Memory

If the user wants persistence, propose an update to `watchers/statehouse-monitor.md`:
- `Last Checked`
- `Sources Watched`
- `Current Watchlist / Open Items`
- `Current Status`
- `Deadlines / Hearings / Effective Dates`
- `Municipal Impact Notes`
- `Recent Changes`
- `Next Check`
- `Notes / Verification Needed`

Use the canonical starter template at `templates/watchers/statehouse-monitor.md` when creating a new monitor file. Preserve the template headings when updating existing memory so future runs can compare the same fields reliably.

Show the update before writing. Keep it factual and public-record-adjacent; do not include campaign strategy, private political whip counts, privileged legal advice, or confidential staff deliberations.

## Output Format

```markdown
# Statehouse Monitor Report

**Municipality**: [Name]
**State**: [State]
**Run Date**: [Date/time]
**Watch Scope**: [Full session / topics / named bills]

## Source and Confidence Notes
- State reference: [state-reference/XX last_verified YYYY-MM-DD / stale / unavailable]
- Current sources checked: [state legislature / agency / municipal league / uploaded source]
- Prior memory source: [watchers/statehouse-monitor.md / prior scan / none]
- Claims requiring verification: [short list]

---

## Alerts

### 🔴 Alert Now
| Item | Change | Municipal Impact | Deadline | Next Step |
|------|--------|------------------|----------|-----------|
| [Bill/rule] | [Status change] *(Confidence: [High/Medium/Low]; provenance: [source])* | [Mandate/preemption/fiscal/etc.] | [Date] | [Action] |

### 🟡 Monitor
| Item | Status | Watch Reason | Next Check |
|------|--------|--------------|------------|
| [Bill/rule] | [Status] *(Confidence: [High/Medium/Low]; provenance: [source])* | [Reason] | [Date/cadence] |

### 🟢 Track Only
- [Item]: [Brief status]

---

## Changes Since Last Run
| Item | Prior Status | Current Status | Significance |
|------|--------------|----------------|--------------|
| [Bill/rule] | [Prior] | [Current] | [🔴/🟡/🟢] |

## Upcoming Deadlines
| Date | Item | Action Needed |
|------|------|---------------|
| [Date] | [Bill/rule/grant/comment period] | [Action] |

## Recommended Follow-Up
- [ ] [Run intergovernmental-scan on topic X]
- [ ] [Ask municipal attorney/staff to verify preemption or mandate conclusion]
- [ ] [Prepare council update or testimony draft]

## Watcher Memory Update
<!-- Include only if user wants local persistence. Show before writing. -->
- Last Checked: [date/time]
- Sources Watched: [official sources checked]
- Current Watchlist / Open Items: [topic, bill, rule, or deadline watchlist updates]
- Current Status: [source-tagged status]
- Deadlines / Hearings / Effective Dates: [dates to track]
- Municipal Impact Notes: [decision-relevant impact notes]
- Next Check: [date/cadence]

## Analysis Boundaries

*This monitor report was produced by a single AI instance using sources available at run time. Legislative and regulatory status can change rapidly.*

**Items requiring verification before local action:**
- [Bill status, amendment text, or effective date]
- [Preemption, home-rule, or mandate conclusion]
- [Fiscal estimate, grant match, or implementation burden]
```

## Recommended Automation Pattern

Run this monitor:
- Weekly during legislative session
- Daily during the final two weeks of session or veto/session deadlines
- Monthly off-session for agency rules, federal notices, and municipal league updates
- Topic-specific daily checks when a bill is on a hearing or floor calendar

Automation prompt:

```text
Run /municipal-governance:statehouse-monitor for [municipality/state]. Watch [topics/bills/sources] for changes since the prior monitor log. Verify current status against official legislature, agency, federal, or municipal league sources where available. Flag mandates, preemption, deadlines, fiscal exposure, and advocacy windows. Include confidence/provenance, state-reference freshness, and no external sends or file updates without confirmation.
```

## Skills Referenced

- `intergovernmental-relations` - home rule, preemption, mandates, grants, and intergovernmental context
- `intergovernmental-scan` - deeper scan report for significant findings
- `policy-evaluation` - impact assessment and advocacy recommendation structure
- `public-finance` - fiscal exposure, mandates, and grant-match analysis
- `municipal-code-analysis` - local code response when state/federal action requires implementation

## Related Skills

- `grant-radar` - funding opportunities discovered during monitoring
- `policy-research` - decision-focused research on significant proposals
- `agenda-watcher` - flags local agenda items responding to statehouse changes
- `skill-qa` - reviews freshness and provenance handling

## Notes

- Never treat a bill summary as a substitute for current bill text when recommending action.
- Distinguish introduced, passed, enacted, and effective.
- Clearly separate municipal league analysis from official legislative status.
- Advocacy recommendations should be grounded in local impact and marked for user confirmation before any external communication.
