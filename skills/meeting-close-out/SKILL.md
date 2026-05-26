---
description: >
  This skill should be used after a council or committee meeting to record
  what happened — votes, decisions, deferred items, and observations.
  Triggers include any mention of meeting close-out, meeting recap,
  post-meeting notes, recording meeting results, what happened at the
  meeting, or updating the briefing book after a meeting.
---

# Meeting Close-Out

Record the outcomes of a council or committee meeting into the briefing book for longitudinal continuity across sessions.

## Trigger

User invokes `/municipal-governance:meeting-close-out`

## Purpose

This is an **input skill**, not an analysis skill. Its job is to capture what happened at a meeting so that future skill runs (meeting-prep, policy-research, etc.) can reference real decisions, votes, and context instead of starting from zero.

The close-out takes 5-10 minutes and creates the institutional memory that makes every subsequent analysis better.

## Inputs

- Which meeting (date, type) — or the user just says "tonight's meeting"
- Meeting outcomes (votes, deferrals, key discussion points)
- User can provide: their own notes, a rough summary, or answer guided questions

## Workflow

### 0. Identify the Meeting

Check `municipal.local.md` for meeting schedule to confirm which meeting this is. If ambiguous, ask:

**"Which meeting are we closing out?"** — Regular session, committee of the whole, special meeting, etc.

If the user already ran `meeting-prep` or `agenda-synthesis` for this meeting, load that session log entry from `briefing-book.md` to pre-populate the agenda items. This means the user only needs to supply *what changed* from the briefing — not re-describe the whole agenda.

### 1. Capture Decisions

Walk through the agenda items (from the briefing if available, or from scratch):

For each action item, ask or confirm:
- **Vote result**: Passed/failed, vote count, and roll call if the user knows it
- **Amendments**: Any changes to the item as presented
- **Key discussion**: 1-2 sentences on what drove the debate (not a transcript — just the substance)

For consent agenda:
- **Pulled items**: Any items removed for separate discussion (and their outcome)
- If nothing pulled: "Consent approved as presented"

For public hearings:
- **Testimony summary**: Themes from public comment (support/opposition/concerns raised)
- **Action taken**: Continued, closed, decision made

### 2. Capture Deferred/Tabled Items

- What was deferred and why
- Expected return date if stated
- Any direction given to staff

### 3. Capture Follow-Ups

- Staff assignments with deadlines
- Items coming back to a future meeting
- Information requests from council members

### 4. Capture Surprises

Ask: **"Anything happen that wasn't in the briefing or that you didn't expect?"**

This is valuable because it calibrates future briefings — if the plugin consistently misses certain types of developments, that's signal.

### 5. Council Dynamics (Optional)

If the user volunteers observations about member positions or voting patterns, record them in the Council Dynamics section. Don't push for this — it's sensitive and the user will share it if they want to.

If the user provides roll call votes, note any votes that diverge from previously observed patterns.

### 6. Update Briefing Book

Write the close-out entry to `briefing-book.md`:

1. **Meeting Record section**: Add the full close-out entry (reverse chronological)
2. **Active Items section**:
   - Update status of existing tracked items based on votes/deferrals
   - Add new items that emerged (new referrals, first readings, staff directives)
   - Remove items that are fully resolved (passed and implemented, or withdrawn)
3. **Council Dynamics section**: Update if new voting pattern data was provided

Before writing, treat `briefing-book.md` as potentially public-record-adjacent. Do not record closed-session content, privileged legal advice, confidential constituent casework, private personnel details, campaign strategy, or personal political notes unless the user explicitly requests a separate non-governmental note outside the briefing book.

## Output Format

Display the close-out summary to the user before writing to the briefing book, so they can correct anything:

```markdown
# Meeting Close-Out
**Meeting**: [Type] — [Date]
**Attendance**: [X of Y members present]

## Decisions

| Item | Action | Vote | Notes |
|------|--------|------|-------|
| [Item title] | Passed / Failed / Deferred / Received | [X-Y] or [Unanimous] | [Key detail] |

## Discussion Highlights
- [Item]: [1-2 sentence summary of substantive debate]

## Deferred Items
- [Item]: Deferred to [date/TBD]. Reason: [brief]. Staff directed to [action].

## Follow-Ups
| What | Who | Due |
|------|-----|-----|
| [Action] | [Staff/Dept] | [Date or timeframe] |

## Surprises / Divergence from Briefing
- [What happened that wasn't anticipated]

---

**Briefing book updated.** [X] active items updated, [Y] new items tracked, [Z] items resolved.
```

After the user confirms (or corrects), write to `briefing-book.md`.

## Skills Referenced

- `parliamentary-procedure` — interpreting procedural outcomes (tabled vs. deferred, motion types)
- `council-communication` — understanding amendment conventions and motion language

## Notes

- This skill is conversational, not analytical. Meet the user where they are — if they give a quick "bag ban passed 5-4, everything else was routine," that's enough. Don't interrogate.
- If the user provides detailed notes, capture the detail. If they're brief, capture what they give and move on. Any close-out is better than no close-out.
- Roll call votes are the most valuable data point for the Council Dynamics section — encourage the user to include them when they know them, but don't require it.
- If a prior meeting-prep or agenda-synthesis session log exists for this meeting, reference it to show what the plugin predicted vs. what happened. This builds trust and calibrates future analyses.
- Keep government meeting records separate from campaign or personal political notes. The briefing book should capture official actions, public discussion themes, and follow-ups, not electioneering strategy.
- The close-out should take 5-10 minutes max. If it's taking longer, the user is providing too much detail — that's great, but keep the briefing book entries concise. Store the detail in the session log, keep the meeting record tight.

## Related Skills

- `meeting-prep` — the primary consumer of close-out data (next meeting's briefing references prior close-outs)
- `agenda-synthesis` — lighter-weight consumer (flags items that appeared in prior meetings)
- `policy-research` — references close-out data when researching topics council has previously considered
