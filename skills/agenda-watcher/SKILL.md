---
description: >
  This skill should be used when the user wants to monitor upcoming municipal
  agendas, agenda packets, addenda, revised meeting notices, or newly posted
  staff reports. Triggers include agenda watcher, watch agendas, monitor
  upcoming meetings, alert me when a packet posts, check for agenda changes,
  agenda addendum, or when the user wants recurring meeting-agenda awareness.
---

# Agenda Watcher

Monitor upcoming agenda postings and packet changes so the workspace can flag new decisions before the user has to go looking for them.

## Trigger

User invokes `/municipal-governance:agenda-watcher`

## Purpose

This is a watcher workflow: it can run manually or as the prompt for a scheduled automation. It does not replace `agenda-synthesis` or `meeting-prep`; it decides whether there is something new worth synthesizing or preparing for.

## Inputs

- Meeting body or bodies to watch
- Look-ahead window (default: next 14 days)
- Agenda source: public agenda URL, uploaded packet, `document-management` when authenticated, or `agenda-management` when available
- Optional prior watcher log from `watchers/agenda-watcher.md` or project notes
- Alert preferences, if the user wants a draft notification

## Workflow

### 0. Scope the Watch

Ask only what is needed:

1. **"Which bodies should I watch?"** - council, committee of the whole, planning commission, zoning board, finance committee, or all configured bodies.
2. **"How far ahead should I look?"** - default to the next 14 days if the user does not specify.
3. **"What should count as an alert?"** - new packet posted, agenda changed, addendum posted, high-dollar item, ordinance, public hearing, executive session, or anything new.

If the user asks to set this up as a recurring watcher, produce a self-contained automation prompt and recommended cadence, but do not create, send, post, or subscribe to anything outside the workspace without explicit confirmation.

### 1. Load Municipal Context

Read `municipal.local.md` for:
- Municipality name and state
- Meeting schedule and location
- Agenda management public URL
- Council/committee structure
- Fiscal impact thresholds
- Policy priorities
- Clerk or agenda contact, if listed

If `municipal.local.md` is missing or incomplete, proceed from the user's source links or uploads and note the missing local context.

### 2. Load State Reference and Check Freshness

If the watch report makes claims about agenda posting deadlines, Open Meetings Act notice, public-hearing requirements, executive-session authority, or final-action limits, read `state-references/{STATE}.md`.

Check `last_verified`, `freshness_window_days`, and `verified_against`. If the reference is stale, missing, or lacks metadata, treat legal/procedural guidance as background only and mark it for clerk or attorney verification.

### 3. Retrieve Current Agenda Sources

Use the best available source:
- `agenda-management` if a future agenda connector is installed
- `document-management` when authenticated and the agenda packet is stored there
- Public agenda URL from `municipal.local.md`
- Uploaded packet, notice, staff report, or addendum

If a connector is unavailable or unauthenticated, use public sources and uploaded materials. Do not imply that an agenda system, staff report repository, or clerk record was reviewed unless it was actually retrieved.

### 4. Compare Against Prior Memory

Look for prior context in this order:
1. `watchers/agenda-watcher.md` in the project workspace
2. `briefing-book.md`
3. `meeting-notes/`
4. The current conversation context

Compare:
- New meetings posted
- Packet now available where only an agenda existed before
- Agenda item added, removed, renamed, or moved from discussion to action
- Addendum, revised agenda, or revised packet posted
- Staff report or attachment added
- Fiscal amount or contract term changed
- Public-hearing, ordinance, executive-session, or closed-session label changed

When no prior memory is available, produce a baseline watch report instead of pretending changes were detected.

### 5. Classify Watch Findings

Use three tiers:

🔴 **Alert Now**
- Agenda or packet posted for a meeting within 7 days and not yet synthesized
- Addendum or revised agenda changes an action item
- New ordinance, code amendment, public hearing, large fiscal item, vendor contract, or executive session appears
- State-law or local-procedure deadline may be close

🟡 **Monitor**
- Packet not yet posted for a scheduled meeting
- Item appears likely significant but lacks staff report or fiscal detail
- Staff report or attachment is missing
- Meeting notice is posted but agenda details are incomplete

🟢 **No Action**
- No new agenda activity
- Routine packet posted with no threshold flags
- Previously watched item unchanged

For decision-relevant claims, include confidence and provenance inline. Tag meeting dates, posting status, item changes, fiscal thresholds, public-hearing/procedural flags, and recommended next actions with sources such as `agenda URL`, `packet p.4`, `document-management`, `state-reference/IL last_verified 2026-03-01`, `prior watcher log`, or `model comparison`.

### 6. Recommend Follow-Up

For each flagged item, recommend the next workflow:
- `agenda-synthesis` for a quick packet summary
- `meeting-prep` for consequential meetings or undecided votes
- `analyze-ordinance` for code amendments
- `budget-review` for major fiscal items
- `vendor-evaluate` for contracts or renewals

If the user wants a notification drafted, prepare it as a draft only. Do not send email, post to chat, update a project tracker, or sync externally without explicit user confirmation in the current session.

### 7. Update Local Watcher Memory

If running inside a project workspace and the user wants persistence, propose an update to `watchers/agenda-watcher.md` with:
- `Last Checked`
- `Sources Watched`
- `Upcoming Meetings`
- `Current Watchlist / Open Items`
- `Current Agenda/Packet Status`
- `New or Changed Items`
- `Items Needing Follow-Up Workflows`
- `Missing Materials`
- `Recent Changes`
- `Next Check`
- `Notes / Verification Needed`

Use the canonical starter template at `templates/watchers/agenda-watcher.md` when creating a new watcher file. Preserve the template headings when updating existing memory so future runs can compare the same fields reliably.

Show the proposed update before writing. Treat watcher logs as public-record-adjacent: do not include campaign strategy, privileged legal advice, closed-session substance, confidential personnel details, or private constituent information.

## Output Format

```markdown
# Agenda Watcher Report

**Municipality**: [Name]
**Run Date**: [Date/time]
**Watch Window**: [Dates]
**Bodies Watched**: [List]

## Source and Confidence Notes
- Agenda source checked: [public URL / agenda-management / document-management / upload]
- Prior memory source: [watchers/agenda-watcher.md / briefing-book.md / none]
- State reference: [state-reference/XX last_verified YYYY-MM-DD / stale / unavailable]
- Missing or unverified materials: [list or "none identified"]

---

## Alerts

### 🔴 Alert Now
| Meeting | Change | Why It Matters | Next Step |
|---------|--------|----------------|-----------|
| [Date/body] | [New packet/addendum/item change] *(Confidence: [High/Medium/Low]; provenance: [source])* | [Reason] | [agenda-synthesis / meeting-prep / other] |

### 🟡 Monitor
| Meeting | Status | Watch Reason | Check Again |
|---------|--------|--------------|-------------|
| [Date/body] | [Packet pending / missing attachment] | [Reason] | [Date/cadence] |

### 🟢 No Action
- [Meetings or sources checked with no material change]

---

## New or Changed Items

### [Meeting Date] - [Body]
| Item | Prior State | Current State | Attention |
|------|-------------|---------------|-----------|
| [Item #/title] | [Prior] | [Current] *(Confidence: [High/Medium/Low]; provenance: [source])* | [🔴/🟡/🟢] |

## Recommended Follow-Up
- [ ] [Run agenda-synthesis for meeting X]
- [ ] [Ask clerk/staff for missing packet Y]
- [ ] [Watch again on date/time]

## Draft Notification
<!-- Include only if the user asked for one. Do not send without confirmation. -->
[Short draft alert for the user/team]

## Watcher Memory Update
<!-- Include only if user wants local persistence. Show before writing. -->
- Last Checked: [date/time]
- Sources Watched: [URLs or locations]
- Current Watchlist / Open Items: [meetings/items still being watched]
- Current Agenda/Packet Status: [meeting/date/version]
- New or Changed Items: [source-tagged changes]
- Items Needing Follow-Up Workflows: [open items]
- Missing Materials: [items to verify]
- Next Check: [date/cadence]

## Analysis Boundaries
<!-- Include when the report flags procedural/legal risk, public hearing requirements, executive-session issues, or major fiscal/code items. -->

*This watcher report compares available sources from a single AI pass. It was not independently verified by the clerk, attorney, or staff.*

**Items requiring verification before action:**
- [Posting deadline, public-hearing, executive-session, or final-action conclusion]
- [Fiscal threshold or contract authority flag]
- [Agenda change inferred from incomplete or inconsistent sources]
```

## Recommended Automation Pattern

For recurring use, run this watcher:
- Weekly during normal periods
- 3-5 days before regular council meetings
- Daily during the 48-72 hours before high-stakes meetings
- Immediately after known agenda posting deadlines, if local practice is known

Automation prompt:

```text
Run /municipal-governance:agenda-watcher for [municipality]. Watch [bodies] for the next [window]. Check configured public agenda sources and available document-management sources. Compare against prior watcher memory if present. Report only new or changed agenda materials, missing packets, and items that should trigger agenda-synthesis, meeting-prep, analyze-ordinance, budget-review, or vendor-evaluate. Include confidence/provenance and do not send external notifications or update files without confirmation.
```

## Skills Referenced

- `agenda-synthesis` - quick synthesis once a packet is found
- `meeting-prep` - deeper preparation for flagged meetings
- `open-meetings-foia` - notice, public-meeting, public-records, and executive-session caution
- `public-finance` - fiscal threshold flagging
- `municipal-code-analysis` - ordinance/code amendment follow-up

## Related Skills

- `meeting-prep` - use when a watched agenda becomes decision-relevant
- `meeting-close-out` - feeds future watcher memory after the meeting
- `council-follow-up-tracker` - tracks assignments that emerge from agenda items
- `skill-qa` - checks watcher safety, provenance, and connector assumptions

## Notes

- A watcher finding is not a legal conclusion. It is an early-warning signal.
- Do not overstate source coverage. If only the public agenda page was checked, say that.
- Do not assume packet absence means the municipality failed to post; sources may be unavailable or organized differently.
- If an agenda labels an executive session, record only the public statutory purpose and agenda wording. Do not seek or summarize closed-session substance.
- Keep watcher memory factual and minimal so it remains useful if treated as a public record.
