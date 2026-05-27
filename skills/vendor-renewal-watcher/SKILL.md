---
description: >
  This skill should be used when the user wants to monitor municipal vendor
  contracts for renewal deadlines, auto-renewal windows, notice periods,
  price escalations, data-export deadlines, or procurement review triggers.
  Triggers include vendor renewal watcher, contract watcher, renewal alert,
  auto-renewal, notice window, vendor contract calendar, or watch our SaaS renewals.
---

# Vendor Renewal Watcher

Track vendor contract renewal windows and prepare the municipality before lock-in, escalation, or procurement deadlines arrive.

## Trigger

User invokes `/municipal-governance:vendor-renewal-watcher`

## Purpose

This watcher turns vendor contracts into an actionable renewal calendar. It is not a substitute for `vendor-evaluate`; it tells the user when a deeper evaluation should happen and what evidence is needed.

## Inputs

- Vendor contracts, staff reports, renewal notices, purchase orders, or contract register
- Optional existing vendor inventory from project workspace
- Watch horizon (default: next 180 days)
- Alert thresholds: renewal window, dollar amount, escalation percentage, data ownership risk, or strategic vendor category

## Workflow

### 0. Scope the Watch

Ask only what is needed:

1. **"Are we watching one contract, a vendor category, or the whole vendor inventory?"**
2. **"How far ahead should renewal alerts fire?"** - default to 180, 90, 60, and 30 days before notice deadline when dates are known.
3. **"What matters most?"** - auto-renewal, cost increase, data ownership, service quality, open-source alternatives, council approval, or procurement compliance.

If the user asks for a recurring watcher, produce a self-contained automation prompt and recommended cadence. Do not contact vendors, send notices, or update external systems without explicit confirmation.

### 1. Load Municipal Context

Read `municipal.local.md` for:
- Contract authority thresholds
- Fiscal impact thresholds
- Procurement ordinance notes
- Current technology vendors and integrations
- IT staffing/capacity
- Policy priorities related to transparency, digital services, or data control

### 2. Load State Reference and Local Procurement Rules

Read `state-references/{STATE}.md` before making procurement, competitive bidding, cooperative purchasing, public-records, or contract-authority claims.

Check `last_verified`, `freshness_window_days`, and `verified_against`. If stale, missing, or lacking metadata, use it as background only and mark procurement conclusions for staff, attorney, or finance verification.

Use `municipal-code` when available to check local purchasing code, contract authority, and approval thresholds. If unavailable, ask for local code excerpts or mark local procurement conclusions as unverified.

### 3. Retrieve Contract Sources

Use the best available source:
- Uploaded contracts, renewals, staff reports, RFPs, or purchase orders
- `document-management` when authenticated
- Vendor inventory or contract register in the project workspace
- Council agenda materials for original approval or renewal

If documents are missing, create a missing-contracts checklist. Do not infer renewal dates, notice periods, or escalation terms unless the source states them.

### 4. Extract Renewal Signals

For each contract, capture:
- Vendor and product/service
- Contract term start/end
- Renewal type: explicit renewal, auto-renewal, month-to-month, or unknown
- Notice deadline and required notice method
- Annual cost, term value, and escalation formula
- Cancellation or termination rights
- Data export, transition assistance, and post-termination access
- Council approval or procurement threshold triggers
- Service-level, breach, or cure provisions
- Key integrations and operational dependencies

Tag every date, dollar amount, notice window, and threshold with confidence/provenance.

### 5. Compare Against Prior Memory

Look for prior context in:
1. `watchers/vendor-renewal-watcher.md`
2. `research/` or prior `vendor-evaluate` reports
3. `documents/contracts/` if the project workspace has one
4. Council agenda or meeting notes

Detect:
- Renewal window newly inside alert horizon
- Notice deadline changed or clarified
- Cost escalation now known or higher than expected
- New renewal notice or amendment received
- Vendor category now has feasible alternatives
- Missing contract details that block informed renewal review

If no prior memory exists, produce a baseline renewal calendar.

### 6. Classify Renewal Risk

🔴 **Act Before Deadline**
- Auto-renewal notice deadline within 90 days
- Contract value exceeds council/procurement threshold
- Escalation, cancellation fee, or term extension creates material fiscal exposure
- Data export or transition rights are weak or missing
- Critical system with no replacement lead time

🟡 **Plan Review**
- Renewal within 6-12 months
- Cost increase, service concerns, or strategic category needs evaluation
- Contract terms are incomplete or unclear
- Alternatives research would help before negotiation

🟢 **Track**
- Renewal more than 12 months away
- Low-dollar/routine contract with clear termination terms
- Recently evaluated with no material changes

### 7. Route Follow-Up

Recommend:
- `vendor-evaluate` for renewal decisions or lock-in concerns
- `budget-review` for fiscal impact or escalation
- `municipal-code-analysis` for procurement/local approval requirements
- `open-meetings-foia` for vendor-held data and records implications
- `agenda-watcher` if renewal should appear on an upcoming agenda

Draft vendor questions, staff information requests, or council agenda questions if useful. Do not send them without explicit confirmation.

### 8. Update Local Watcher Memory

If the user wants persistence, propose an update to `watchers/vendor-renewal-watcher.md`:
- Contract inventory row
- Renewal/notice dates
- Risk tier
- Missing documents
- Recommended next review date
- Follow-up owner/status if known

Show the update before writing. Keep watcher records factual and avoid privileged legal advice, confidential negotiation strategy, or campaign/political notes.

## Output Format

```markdown
# Vendor Renewal Watcher Report

**Municipality**: [Name]
**Run Date**: [Date/time]
**Watch Horizon**: [Dates]
**Scope**: [One vendor / category / inventory]

## Source and Confidence Notes
- Contract sources reviewed: [uploaded / document-management / contract register / agenda packet]
- Prior memory source: [watchers/vendor-renewal-watcher.md / prior vendor-evaluate / none]
- State procurement reference: [state-reference/XX last_verified YYYY-MM-DD / stale / unavailable]
- Local procurement source: [municipal-code / uploaded code excerpt / not verified]
- Missing documents: [list or "none identified"]

---

## Renewal Alerts

### 🔴 Act Before Deadline
| Vendor | Deadline | Risk | Next Step |
|--------|----------|------|-----------|
| [Vendor/product] | [Notice/renewal date] *(Confidence: [High/Medium/Low]; provenance: [source])* | [Auto-renewal/cost/data/procurement] | [Action] |

### 🟡 Plan Review
| Vendor | Renewal Timing | Watch Reason | Suggested Review |
|--------|----------------|--------------|------------------|
| [Vendor/product] | [Date/window] | [Reason] | [Date/workflow] |

### 🟢 Track
- [Vendor/product]: [brief status]

---

## Renewal Calendar
| Vendor | Product/Service | Term End | Notice Deadline | Annual Cost | Attention |
|--------|-----------------|----------|-----------------|-------------|-----------|
| [Vendor] | [Service] | [Date] | [Date/unknown] | $[X] | [🔴/🟡/🟢] |

## Missing Information
- [Contract or clause needed]
- [Staff/vendor question needed]

## Recommended Follow-Up
- [ ] [Run vendor-evaluate on Vendor X by date]
- [ ] [Ask staff for current contract/amendment]
- [ ] [Confirm procurement threshold with finance/procurement/attorney]
- [ ] [Request data export terms before renewal negotiation]

## Draft Questions
<!-- Include only if useful. Do not send without confirmation. -->
- [Question for staff/vendor/legal]

## Analysis Boundaries

*This watcher report extracts contract signals from available documents in a single AI pass. It does not replace attorney, finance, procurement, or IT review.*

**Items requiring verification before action:**
- [Renewal or notice deadline]
- [Procurement threshold or council approval requirement]
- [Termination, auto-renewal, or data ownership interpretation]
- [Cost projection or escalation assumption]
```

## Recommended Automation Pattern

Run this watcher:
- Monthly for the full vendor inventory
- Weekly when any 🔴 renewal is inside 90 days
- After each council meeting where vendor contracts or amendments are approved
- Immediately when a renewal notice, amendment, or price increase arrives

Automation prompt:

```text
Run /municipal-governance:vendor-renewal-watcher for [municipality]. Watch contracts and vendor inventory for renewal, auto-renewal, notice, escalation, procurement, and data-export deadlines in the next [horizon]. Compare against prior watcher memory if present. Include confidence/provenance and state/local procurement freshness. Do not contact vendors, send notices, update external systems, or write watcher memory without confirmation.
```

## Skills Referenced

- `vendor-assessment` - lock-in, data portability, and build-vs-buy risk
- `vendor-alternatives` - replacement tiers and open-source alternatives
- `vendor-evaluate` - deeper renewal evaluation
- `public-finance` - fiscal impact and budget classification
- `municipal-code-analysis` - procurement and contract authority lookup
- `open-meetings-foia` - vendor-held public records and data access

## Related Skills

- `agenda-watcher` - catches upcoming renewal agenda items
- `budget-review` - analyzes renewal fiscal impact
- `grant-radar` - tracks funding that may affect vendor replacement or implementation
- `skill-qa` - checks procurement, provenance, and external-action gates

## Notes

- Never recommend missing a contractual notice deadline as leverage.
- Never recommend breaking a current contract; frame alternatives as future renewal planning.
- Treat negotiation strategy and attorney advice as sensitive. Keep local watcher memory factual.
- If a contract has an auto-renewal clause but no source-confirmed notice date, mark the date unknown and prioritize document retrieval.
