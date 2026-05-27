---
description: >
  This skill should be used when the user needs a comprehensive briefing
  for an upcoming council or committee meeting. Triggers include any
  mention of meeting preparation, meeting briefing, council meeting
  preview, committee meeting prep, or when the user needs to prepare
  for a specific upcoming meeting with full analysis of agenda items,
  procedural requirements, and deliberation questions.
---

# Meeting Preparation

Generate a comprehensive briefing for an upcoming council or committee meeting.

## Trigger

User invokes `/municipal-governance:meeting-prep`

## Inputs

- Meeting date and type (regular session, committee, special meeting, workshop)
- Agenda packet (file upload, `document-management` when authenticated, or `agenda-management` when available)
- Specific items of interest (optional)

## Workflow

### 0. Scope the Briefing

Before diving into the full agenda, ask the user a few quick questions to focus the analysis:

1. **"How many items on this agenda need your close attention?"** — The user often knows which 2-3 items are the real decisions. Everything else can get consent-level treatment.
2. **"Any items you're already up to speed on?"** — Skip deep analysis on items the user has background on. Mention them briefly for completeness.
3. **"Are there specific votes you're undecided on?"** — These get the deepest treatment: code cross-references, fiscal analysis, stakeholder perspectives, and deliberation questions.

If the user says "just give me everything," proceed with full analysis. If they identify focus items, use this tiered approach:

| User says | Treatment |
|-----------|-----------|
| "Focus on this" | Full deep-dive: code analysis, fiscal impact, stakeholder perspectives, deliberation questions |
| "I know about this one" | One-line summary, flag only if something unexpected surfaces |
| No guidance on an item | Standard treatment: staff rec, fiscal impact, key points |
| Consent agenda items | Brief table unless user asks to pull one |

This step should take under a minute. The goal is to match the briefing depth to where the user actually needs help.

### 1. Load Municipal Context

Check `municipal.local.md` for:
- Council structure and committees
- Meeting schedule and procedures
- Policy priorities
- Key contacts

### 2. Load State Reference and Check Freshness

When the briefing includes Open Meetings Act, public-hearing, quorum, voting, executive-session, notice, or other procedural compliance guidance, read `state-references/{STATE}.md` using the state from `municipal.local.md`.

Check `last_verified`, `freshness_window_days`, and `verified_against`. If the state reference is missing or stale, treat state-law guidance as background only and mark procedural conclusions as requiring staff, clerk, or attorney verification before the meeting.

### 3. Retrieve Agenda and Materials

Retrieve materials from the best available source:
- `document-management` when Box access is authenticated
- `agenda-management` if a future agenda connector is installed
- Uploaded agenda packets, staff reports, or links from the user

If connectors are unavailable or unauthenticated, proceed from uploaded documents and clearly list missing materials. Do not assume an item is routine merely because supporting documents were not available.

Documents to look for:
- Official agenda
- Staff reports for each item
- Supporting documents (contracts, plans, etc.)
- Prior meeting minutes (for context)

### 4. Agenda Overview

Create a quick-reference summary:

| # | Item | Type | Staff Rec | Fiscal Impact | Attention Level |
|---|------|------|-----------|---------------|-----------------|
| 1 | [Title] | [Action/Info/Hearing] | [Approve/Deny/None] | [$X] | [🟢/🟡/🔴] |

### 5. Categorize Items

**Consent Agenda**
- Identify items on consent
- Flag any that may warrant removal for separate discussion
- Note any with fiscal impact above threshold
- **Best practices**: Publish materials far enough in advance for review; any member may typically pull an item for separate discussion without a second or vote if local rules allow; typical items include minutes approval, routine contracts, and bill payments. Treat timing and pull rules as local-procedure questions unless verified in state law, municipal code, or council rules.

**Public Hearings**
- Required notice verification
- Key issues expected
- Quasi-judicial considerations

**Action Items**
- Items requiring a vote
- Supermajority requirements
- Potential controversy

**Informational Items**
- Presentations
- Reports
- Updates

### 6. Synthesize Each Item

For each non-consent item, provide:

**What is Being Asked**
- Specific action requested
- Motion language (if provided)
- Decision options available

**Staff Recommendation**
- Summary of staff position
- Key supporting rationale
- Alternatives considered

**Fiscal Impact**
- Direct costs
- Budget line item
- Ongoing vs. one-time

**Code Implications**
- Any code changes involved
- Trigger ordinance analysis skill if needed

**Key Decision Points**
- Core policy question
- Trade-offs to consider
- Stakeholder perspectives

**Background Context**
- How this item came to council
- Related prior decisions
- Timeline/deadline pressures

### 7. Flag Items Requiring Special Attention

Automatically flag items with:

🔴 **Critical Attention**
- Significant fiscal impact (>threshold from municipal.local.md)
- Code amendments (trigger ordinance analysis)
- Legal risk identified
- Known community controversy
- Supermajority or special voting requirements

🟡 **Elevated Attention**
- Moderate fiscal impact
- Policy direction questions
- Implementation complexity
- Time-sensitive matters

🟢 **Routine**
- Standard approvals
- Informational updates
- Consent-appropriate items

### 8. Check Procedural Requirements

**Open Meetings Compliance**
- Proper notice given?
- Agenda posted timely?
- Notice content adequate?

**Public Hearings**
- Required hearings scheduled?
- Notice published per requirements?
- Quasi-judicial procedures needed?

**Quorum and Voting**
- Expected attendance
- Recusal/abstention anticipated
- Supermajority items identified

### 9. Generate Meeting Briefing

Produce comprehensive briefing document. For decision-relevant claims, include confidence and provenance inline. Tag fiscal figures, code impacts, procedural requirements, and legal-risk flags with concise sources such as `agenda packet p.12`, `staff report`, `municipal-code`, `state-reference/IL last_verified 2026-03-01`, `clerk rules`, `web - verify`, or `model inference`.

## Output Format

*Omit any section that doesn't apply to this meeting. For meetings with few agenda items, a shorter briefing is better than padding thin sections.*

```markdown
# Meeting Briefing
**[Meeting Type]**: [City Council Regular Meeting / Committee Name / etc.]
**Date**: [Date and Time]
**Location**: [Location]

## Quick Reference

### Attention Summary
- 🔴 Critical Items: [count]
- 🟡 Elevated Items: [count]
- 🟢 Routine Items: [count]

### Key Numbers
- Total agenda items: [X]
- Public hearings: [X]
- Consent items: [X]
- Total fiscal impact: $[X]

### Source Notes
- Agenda packet source: [uploaded packet / document-management / agenda link]
- Procedural law reference: [state-reference/XX last_verified YYYY-MM-DD / needs verification]
- Missing materials: [list or "none identified"]

---

## Agenda Overview

| # | Item | Action | Fiscal | Attention |
|---|------|--------|--------|-----------|
[Table of all items]

---

## Consent Agenda Review

[List consent items with any flags]

**Items to Consider Removing**:
- [Item X] - [reason]

---

## Public Hearings

### [Item Number]: [Title]
**Required by**: [statute/code reference] *(Confidence: [High/Medium/Low]; provenance: [source])*
**Notice**: [Verified/Check required] *(Confidence: [High/Medium/Low]; provenance: [source])*
**Key Issues**: [Summary]
**Public Comment Expected**: [Yes/No/Unknown]

---

## Action Items

### [Item Number]: [Title] [🔴/🟡/🟢]

**The Ask**: [Clear statement of what council is being asked to do]

**Staff Recommendation**: [Approve/Deny/Direct staff to...]

**Key Points**:
- [Point 1]
- [Point 2]
- [Point 3]

**Fiscal Impact**: $[amount] - [one-time/ongoing] - [funding source] *(Confidence: [High/Medium/Low]; provenance: [source])*

**Code Impact** *(if applicable)*:
- [Current code provision and proposed change] *(Confidence: [High/Medium/Low]; provenance: [source])*

**Questions to Consider** (apply the Standard Deliberation Framework):
1. **Fiscal impact**: What does this cost? One-time vs. recurring? Fund balance impact?
2. **Legal/compliance**: Does the municipality have authority? State law constraints? Liability?
3. **Policy alignment**: Consistent with strategic plan? Conflicts with existing policy?
4. **Community impact**: Who benefits? Who is burdened? Geographic/demographic distribution?
5. **Alternatives**: What other options were evaluated? Why this recommendation?
6. **Timeline**: When does this take effect? Staff capacity to implement?
7. **Risk**: What could go wrong? Reversibility? Political/legal/financial risk?

**Background**: [Brief context]

---

[Repeat for each action item]

---

## Procedural Notes

### Voting Requirements
- Items requiring supermajority: [list]
- Expected recusals: [if known]

### Public Comment
- Scheduled: [yes/no/per rules]
- Time limit: [X minutes per speaker]

### Executive Session
- Scheduled: [yes/no]
- Topics: [if scheduled]
- Record only that an executive session is scheduled and its stated statutory purpose. Do not transcribe closed-session deliberations, privileged legal advice, or confidential personnel/real-estate details into the briefing.

---

## Preparation Checklist

- [ ] Review staff reports for flagged items
- [ ] Prepare questions for [specific items]
- [ ] Review [related documents]
- [ ] [Other preparation tasks]

## Analysis Boundaries
<!-- Include only when the agenda contains 🔴 items involving code amendments, significant fiscal impact, or legal risk. Omit for routine agendas. -->

*The following items received single-instance analysis that may not be sufficient for the decision at hand:*

| Item | What needs verification | Recommended next step |
|------|------------------------|----------------------|
| [🔴 Item X] | [Specific claim — e.g., "preemption analysis assumes home rule authority"] | Attorney review |
| [🔴 Item Y] | [Specific claim — e.g., "fiscal estimate based on staff report, not independently modeled"] | Finance staff confirmation |
| [Procedural issue] | [Specific notice/hearing/voting claim based on stale or incomplete state/local rules] | Clerk or attorney verification |

For complex or contested items, consider escalating to **PolicyAide multi-agent deliberation** before the vote — it stress-tests analysis through structured adversarial debate, surfacing blind spots a single-pass briefing may miss.

---
*Briefing prepared for informational purposes.
Contact [staff contact] with questions.*
```

## Skills Referenced

- `parliamentary-procedure` — quorum, voting requirements, motion procedures
- `open-meetings-foia` — notice compliance, closed session procedures
- `public-finance` — fiscal impact analysis for budget items
- `municipal-code-analysis` — code amendment analysis for ordinance items
- `ethics-conflicts` — flag items where officials may need to recuse

## Depth Modes

The scoping step (Step 0) determines which mode to use:

| Mode | When | What you get |
|------|------|-------------|
| **Focused** | User identifies 2-3 items for deep analysis | Full deep-dive on focus items, one-line summaries for everything else. Fastest useful output. |
| **Standard** | User says "cover it all" or provides no focus | Standard treatment for action items, brief for consent. The default. |
| **Comprehensive** | User says "give me everything" or is preparing for a complex meeting | Deep analysis on all action items, code cross-references, peer comparisons, full deliberation questions. Longest output. |

When in doubt, default to **Standard** and let the user escalate specific items.

## Notes

- **Preparation benchmark**: Plan for approximately 1-2 hours of preparation per hour of meeting for routine agendas; 3-5+ hours for complex packets. Focus preparation time on identifying the 2-3 most consequential items.
- **Advance staff contact**: Contact staff before the meeting to resolve factual ambiguities and get answers to technical questions. This prevents surprises during the meeting and is standard practice recommended by ICMA ("no surprise" rule — the city manager should brief the mayor/chair about the agenda before every meeting).
- Match depth to the mode selected in scoping — don't produce comprehensive output when the user asked for a focused briefing
- For consent agenda items, brief descriptions are always sufficient unless user specifically asks to pull one
- When agenda management tools are not connected, work from uploaded documents
- Treat meeting briefings as potentially public records. Keep campaign strategy, personal political notes, and private constituent details out of official meeting-prep output unless the user explicitly asks for a separate non-governmental note.
- Do not rely on stale or missing state references for Open Meetings, public-hearing, executive-session, or voting guidance; surface the verification need directly
- Cross-reference policy priorities from `municipal.local.md`
- Include relevant context from prior meetings when available

## Related Skills

- `agenda-synthesis` — for a quicker, lighter-weight agenda summary
- `analyze-ordinance` — for deep analysis of any ordinance on the agenda
- `budget-review` — for detailed fiscal analysis of budget items on the agenda
