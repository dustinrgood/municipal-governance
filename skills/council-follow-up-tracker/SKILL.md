---
description: >
  This skill should be used when the user wants to track council follow-ups,
  staff directives, deferred agenda items, promised reports, information
  requests, meeting action items, or open loops after a meeting. Triggers
  include follow-up tracker, council action tracker, track staff follow-ups,
  what is still open, deferred item tracker, promised report, or action item
  monitoring.
---

# Council Follow-Up Tracker

Track public meeting follow-ups so staff directives, deferred items, and promised returns do not disappear between meetings.

## Trigger

User invokes `/municipal-governance:council-follow-up-tracker`

## Purpose

This watcher turns `meeting-close-out` notes into a live open-items list. It focuses on public, official follow-ups: staff assignments, deferred agenda items, promised memos, return dates, and information requests.

## Inputs

- Meeting close-out notes, minutes, agenda packets, or user notes
- Optional `briefing-book.md`, `meeting-notes/`, or `watchers/council-follow-up-tracker.md`
- Watch horizon for due/overdue items
- Optional area of focus: staff reports, ordinances, constituent-facing issues, budget items, vendor renewals, grants

## Workflow

### 0. Scope the Tracker

Ask only what is needed:

1. **"Which follow-ups should I track?"** - one meeting, all open items, one department/topic, or items due before the next council meeting.
2. **"What sources should I use?"** - meeting close-out, approved minutes, agenda packets, user notes, or watcher memory.
3. **"Do you want a status report only, or should I propose updates to the local tracker?"**

If the user asks for recurring monitoring, produce a self-contained automation prompt and recommended cadence. Do not contact staff, send reminders, or update external trackers without explicit confirmation.

### 1. Load Municipal Context

Read `municipal.local.md` for:
- Meeting schedule
- Council/committee structure
- Key staff contacts
- Policy priorities
- Procedural notes

### 2. Load State Reference and Check Freshness

Read `state-references/{STATE}.md` when interpreting open-meetings, minutes, executive-session, public-records, public-hearing, or procedural compliance requirements.

Check `last_verified`, `freshness_window_days`, and `verified_against`. If stale, missing, or lacking metadata, treat procedural guidance as background only and mark it for clerk or attorney verification.

### 3. Retrieve Follow-Up Sources

Use available sources:
- `meeting-close-out` output
- `briefing-book.md`
- `meeting-notes/`
- Approved minutes or meeting video summaries
- Agenda packets and staff reports
- `document-management` when authenticated
- `project-tracking` if a future connector is installed

If sources are missing, produce a missing-source note. Do not invent assignments, due dates, or owners; mark them unknown unless a source states them.

### 4. Extract Open Loops

Capture:
- Staff directives
- Deferred, tabled, or continued items
- Items promised to return at a later meeting
- Requested staff reports, legal memos, fiscal analyses, or public updates
- Constituent-facing commitments made in public meeting context
- Public-hearing continuations
- Grant, vendor, budget, ordinance, or policy follow-ups

Avoid:
- Closed-session substance
- Privileged legal advice
- Confidential personnel, real estate, or litigation strategy
- Private constituent casework details
- Campaign or electioneering strategy

### 5. Compare Against Prior Memory

Look for prior context in:
1. `watchers/council-follow-up-tracker.md`
2. `briefing-book.md`
3. `meeting-notes/`
4. Prior agenda watcher reports
5. Current agenda or packet materials

Detect:
- New follow-up created
- Due date approaching or passed
- Item returned on an agenda
- Item completed, superseded, or withdrawn
- Owner/status changed
- Follow-up needs escalation to another workflow

If no prior memory exists, produce a baseline open-items list.

### 6. Classify Follow-Ups

🔴 **Due / Needs Attention**
- Due before the next meeting or overdue
- Council direction with no owner or due date
- Deferred item with public expectation or statutory deadline
- Follow-up connected to legal, fiscal, public-hearing, grant, vendor, or safety risk

🟡 **Open / Monitor**
- Owner or timeframe exists but not yet due
- Awaiting staff report, committee return, or external response
- Needs source verification

🟢 **Closed / Informational**
- Completed, returned to agenda, adopted, denied, withdrawn, or no further action

For decision-relevant claims, include confidence and provenance inline. Tag owners, due dates, vote outcomes, return dates, statutory/procedural deadlines, and status changes with sources such as `meeting-close-out`, `minutes p.2`, `agenda packet p.14`, `watcher log`, `state-reference/IL last_verified 2026-03-01`, or `model inference`.

### 7. Route Follow-Up

Recommend:
- `meeting-prep` if the item is coming back to a meeting
- `agenda-watcher` if the return date is uncertain
- `analyze-ordinance` for ordinance/code follow-ups
- `budget-review` for fiscal follow-ups
- `vendor-renewal-watcher` for contract follow-ups
- `grant-radar` for grant commitments
- `council-communication` for draft staff questions or public updates

Draft reminders or staff questions only. Do not send them or update external project trackers without explicit user confirmation.

### 8. Update Local Tracker Memory

If the user wants persistence, propose an update to `watchers/council-follow-up-tracker.md`:
- `Last Checked`
- `Sources Watched`
- `Open Follow-Ups`
- `Upcoming Agenda Linkage`
- `Current Watchlist / Open Items`
- `Closed Items`
- `Recent Changes`
- `Next Check`
- `Notes / Verification Needed`

Use the canonical starter template at `templates/watchers/council-follow-up-tracker.md` when creating a new tracker file. Preserve the template headings when updating existing memory so future runs can compare the same fields reliably.

Show the update before writing. Keep tracker memory factual and public-record-adjacent. Do not include private political notes, legal strategy, closed-session substance, confidential personnel details, or private constituent casework.

## Output Format

```markdown
# Council Follow-Up Tracker

**Municipality**: [Name]
**Run Date**: [Date/time]
**Scope**: [All open items / meeting / department / topic]
**Next Meeting**: [Date if known]

## Source and Confidence Notes
- Sources reviewed: [meeting-close-out / minutes / agenda packet / watcher log / document-management]
- Prior memory source: [watchers/council-follow-up-tracker.md / briefing-book.md / none]
- State reference: [state-reference/XX last_verified YYYY-MM-DD / stale / unavailable]
- Missing or unverified sources: [list or "none identified"]

---

## Open Items

### 🔴 Due / Needs Attention
| Item | Owner | Due/Return Date | Status | Next Step |
|------|-------|-----------------|--------|-----------|
| [Follow-up] | [Owner/unknown] | [Date] *(Confidence: [High/Medium/Low]; provenance: [source])* | [Status] | [Action] |

### 🟡 Open / Monitor
| Item | Owner | Timing | Watch Reason |
|------|-------|--------|--------------|
| [Follow-up] | [Owner/unknown] | [Date/window] | [Reason] |

### 🟢 Closed / Informational
- [Item]: [Closed/completed/returned/withdrawn] *(Confidence: [High/Medium/Low]; provenance: [source])*

---

## Changes Since Last Run
| Item | Prior Status | Current Status | Source |
|------|--------------|----------------|--------|
| [Item] | [Prior] | [Current] | [Source] |

## Items Appearing on Upcoming Agendas
- [Item]: [Meeting/date/action expected]

## Recommended Follow-Up
- [ ] [Ask staff for status/date/source]
- [ ] [Run meeting-prep for returning item]
- [ ] [Watch next agenda for item X]
- [ ] [Close completed item in local tracker after confirmation]

## Draft Reminder or Question
<!-- Include only if requested. Do not send without confirmation. -->
[Draft text]

## Watcher Memory Update
<!-- Include only if user wants local persistence. Show before writing. -->
- Last Checked: [date/time]
- Sources Watched: [meeting notes/minutes/packets]
- Open Follow-Ups: [source-tagged open rows]
- Upcoming Agenda Linkage: [return agenda links]
- Current Watchlist / Open Items: [items still monitoring]
- Closed Items: [confirmed closures]
- Next Check: [date/cadence]

## Analysis Boundaries

*This tracker is a single-instance synthesis of available public or user-provided records. It may miss updates not present in the reviewed sources.*

**Items requiring verification before action:**
- [Owner, due date, or return date not stated in an official source]
- [Procedural or statutory deadline]
- [Status inferred from an agenda item rather than confirmed in minutes/staff response]
```

## Recommended Automation Pattern

Run this tracker:
- After each `meeting-close-out`
- 3-5 days before the next regular council meeting
- Weekly for active project or policy follow-ups
- Daily when a time-sensitive public-hearing, grant, vendor, or ordinance item is due

Automation prompt:

```text
Run /municipal-governance:council-follow-up-tracker for [municipality]. Review watcher memory, meeting notes, close-out records, minutes, and available agenda materials for open public follow-ups, deferred items, staff directives, and promised returns. Compare against prior tracker memory. Flag due, overdue, returning, and closed items with confidence/provenance. Do not contact staff, send reminders, update external trackers, or write local files without confirmation.
```

## Skills Referenced

- `meeting-close-out` - creates source material for follow-up memory
- `parliamentary-procedure` - deferred, tabled, continued, and adopted item interpretation
- `council-communication` - draft questions, reminders, and public updates
- `open-meetings-foia` - public-records, minutes, and closed-session boundaries
- `policy-evaluation` - prioritization of follow-ups by policy significance

## Related Skills

- `agenda-watcher` - checks whether open items return to an agenda
- `meeting-prep` - prepares for returning follow-up items
- `vendor-renewal-watcher` - tracks contract-related follow-ups
- `grant-radar` - tracks grant-related commitments and deadlines

## Notes

- This tracker is for official public-business follow-ups, not private political strategy.
- Record only that an executive session occurred or was scheduled and its public statutory purpose. Do not track closed-session substance.
- Keep local tracker entries short, factual, and source-tagged.
- When owner or due date is unclear, ask a neutral status question rather than assigning blame.
