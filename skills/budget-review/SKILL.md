---
description: >
  This skill should be used when the user needs to analyze budget documents,
  fiscal impacts, financial proposals, or budget amendments. Triggers include
  any mention of budget review, budget analysis, fiscal impact, fund balance,
  revenue analysis, expenditure review, budget amendment, or when the user
  needs to understand the financial implications of a municipal action.
---

# Budget Review

Analyze budget items, fiscal impacts, and financial proposals.

## Trigger

User invokes `/municipal-governance:budget-review`

## Inputs

- Budget document(s) or specific item to analyze
- Analysis type (full budget review, specific item, amendment, fiscal impact)
- Comparison data (prior year, proposed vs. adopted, peer communities)
- Specific questions or concerns

## Workflow

### 1. Load Municipal Context

Check `municipal.local.md` for:
- Fiscal year (calendar vs. other)
- General fund size
- Major revenue sources
- Tax limitations (PTELL, etc.)
- Policy priorities
- Fund balance policies

### 2. Load State Reference and Financial Source Context

When the analysis involves levy limits, tax caps, budget adoption deadlines, debt limits, public finance statutes, state aid, or peer-comparison data sources, read `state-references/{STATE}.md` using the state from `municipal.local.md`.

Check `last_verified`, `freshness_window_days`, and `verified_against`. If the state reference is missing, lacks metadata, or is stale, treat state-law and state-data-source guidance as background only and flag those conclusions for finance staff, clerk, or attorney verification.

Identify the source for each major number before analyzing it:
- Adopted or proposed budget
- Annual financial report or audit
- Staff report or budget memo
- State comptroller / state financial database
- User-provided estimate
- Model inference

### 3. Scope the Analysis

Ask the user what they need before proceeding:

**"What are you looking at — the full budget, a specific item, an amendment, or a fiscal impact question?"**

Then confirm depth:

| User says | Analysis mode |
|-----------|--------------|
| "Walk me through the whole budget" | Full budget review — all sections |
| "What's going on with the police department budget" | Specific item — focused on one department or line item, skip full budget sections |
| "There's a budget amendment on the agenda" | Amendment analysis — what's changing, funding source, fund balance impact |
| "What would this proposal cost us" | Fiscal impact — revenue/cost projections, net impact, funding options |
| "Just the highlights" | Executive summary + key numbers + concerns only |

**Full Budget Review**:
- Overall revenue and expenditure trends
- Fund balance analysis
- Major changes from prior year
- Alignment with policy priorities

**Specific Item Analysis**:
- Line item detail
- Historical trends
- Peer comparison
- Cost drivers

**Budget Amendment**:
- What's changing
- Funding source
- Impact on fund balance
- Policy implications

**Fiscal Impact Analysis**:
- For proposed projects/policies
- Revenue projections
- Cost projections
- Net fiscal impact

### 4. Conduct Analysis

**Revenue Analysis**:
- Major revenue sources and trends
- Assumptions validation
- Sensitivity analysis
- One-time vs. recurring
- Economic factors

**Expenditure Analysis**:
- Major cost categories
- Personnel costs (typically 60-80%)
- Operating vs. capital
- Contractual obligations
- Discretionary vs. fixed

**Fund Balance Analysis**:
- Current fund balance
- Policy minimums
- Projected year-end
- Trend over time

**Debt Analysis**:
- Outstanding debt
- Debt service burden
- Debt capacity
- Upcoming issuances

For decision-relevant claims, include confidence and provenance inline. Tag fiscal figures, revenue assumptions, fund balance calculations, debt metrics, peer comparisons, and legal/statutory finance constraints with `High/Medium/Low` and concise sources such as `budget p.14`, `AFR`, `staff memo`, `state-reference/IL last_verified 2026-03-01`, `state comptroller`, `web - verify`, or `model inference`.

### 5. Generate Analysis Report

## Output Format

*Omit any section where data is unavailable. For specific-item analysis, skip full budget sections and focus on the item in question.*

```markdown
# Budget Analysis: [Title/Focus]

**Analysis Date**: [Date]
**Budget Year**: [FY]
**Prepared for**: [Requestor]

## Source and Confidence Notes
- Primary budget source: [document name/page range]
- State finance reference: [state-reference/XX last_verified YYYY-MM-DD / stale / unavailable]
- Peer data source: [state comptroller / ICMA / uploaded comparison / not available]
- Numbers requiring verification: [short list of Medium/Low confidence figures]

---

## Executive Summary

[3-4 paragraph summary of key findings, concerns, and observations]

---

## Budget Overview

### Key Numbers

| Metric | Amount | Change from PY | Confidence / Provenance |
|--------|--------|----------------|-------------------------|
| Total Budget | $[X] | [+/-X%] | [High/Med/Low; source] |
| General Fund | $[X] | [+/-X%] | [High/Med/Low; source] |
| Property Tax Levy | $[X] | [+/-X%] | [High/Med/Low; source] |
| Personnel Costs | $[X] | [+/-X%] | [High/Med/Low; source] |
| Capital Investment | $[X] | [+/-X%] | [High/Med/Low; source] |
| Projected Fund Balance | $[X] | [X% of expenditures] | [High/Med/Low; source] |

### Budget by Fund

| Fund | Revenue | Expenditures | Net |
|------|---------|--------------|-----|
| General | $[X] | $[X] | $[X] |
| [Other funds] | | | |

---

## Revenue Analysis

### Revenue Mix

| Source | Amount | % of Total | Trend |
|--------|--------|------------|-------|
| Property Tax | $[X] | [X%] | [↑/↓/→] |
| Sales Tax | $[X] | [X%] | [↑/↓/→] |
| [etc.] | | | |

### Key Revenue Observations

- [Observation 1]
- [Observation 2]

### Revenue Assumptions

| Assumption | Basis | Risk Level |
|------------|-------|------------|
| [Assumption 1] | [How derived] | [Low/Med/High] |

### Revenue Risks

- [Risk 1]
- [Risk 2]

---

## Expenditure Analysis

### Expenditure by Category

| Category | Amount | % of Total | Change |
|----------|--------|------------|--------|
| Personnel | $[X] | [X%] | [+/-X%] |
| Operating | $[X] | [X%] | [+/-X%] |
| Capital | $[X] | [X%] | [+/-X%] |
| Debt Service | $[X] | [X%] | [+/-X%] |

### Expenditure by Department

| Department | Amount | Change | Notes |
|------------|--------|--------|-------|
| [Dept 1] | $[X] | [+/-X%] | [Notable items] |

### Significant Changes

**Increases over [5%]**:
- [Item]: [Amount] - [Reason]

**Decreases over [5%]**:
- [Item]: [Amount] - [Reason]

**New Programs/Positions**:
- [Description]

---

## Personnel Analysis

| Metric | Count/Amount |
|--------|--------------|
| Total FTEs | [X] |
| Change from PY | [+/-X] |
| Salary Budget | $[X] |
| Benefits Budget | $[X] |
| Benefits as % of Salary | [X%] |

### Position Changes
- New positions: [X]
- Eliminated positions: [X]
- Reclassifications: [X]

---

## Fund Balance Analysis

### General Fund Balance

| Category | Amount |
|----------|--------|
| Beginning Balance | $[X] |
| Revenues | $[X] |
| Expenditures | $[X] |
| Ending Balance | $[X] |
| As % of Expenditures | [X%] |

### Fund Balance Policy
- Minimum policy: [X%]
- Current: [X%]
- Status: [Above/At/Below policy]

### Fund Balance Trend

| Year | Balance | % of Exp |
|------|---------|----------|
| [FY-2] | $[X] | [X%] |
| [FY-1] | $[X] | [X%] |
| [FY] Projected | $[X] | [X%] |

---

## Debt Analysis

### Outstanding Debt
- GO Bonds: $[X]
- Revenue Bonds: $[X]
- Other Obligations: $[X]
- **Total**: $[X]

### Debt Metrics
- Debt per capita: $[X]
- Debt as % of EAV: [X%]
- Debt service as % of budget: [X%]

### Upcoming Debt
- [Planned issuances]

---

## Capital Investment

### Capital Budget Summary
- Total capital: $[X]
- Funded from operations: $[X]
- Funded from debt: $[X]
- Funded from grants: $[X]

### Major Projects
| Project | Amount | Funding | Status |
|---------|--------|---------|--------|
| [Project 1] | $[X] | [Source] | [Year X of Y] |

---

## Policy Alignment

### Priority 1: [From municipal.local.md]
- Budget support: [Strong/Moderate/Weak]
- Relevant investments: [List]

### Priority 2: [From municipal.local.md]
[Same format]

---

## Concerns and Questions

### Financial Concerns
1. [Concern with explanation]
2. [Concern with explanation]

### Questions for Staff
1. [Question]
2. [Question]

### Items Warranting Further Review
1. [Item with reason]
2. [Item with reason]

---

## Peer Comparison (if available)

For peer comparison data, the state reference in `state-references/` identifies state-specific data sources (e.g., Illinois Comptroller's Local Government Warehouse provides Annual Financial Report data from 9,200+ local governments). ICMA's Open Access Benchmarking offers 80 free municipal KPIs for self-service comparison.

Note: GFOA's Distinguished Budget Presentation Award underwent a major overhaul for 2026 — now uses a points-based system (>100 points) evaluated on two axes (information communicated and material type), replacing the legacy four-category mandatory criteria.

| Metric | [City] | Peer Avg | State Avg |
|--------|--------|----------|-----------|
| Per capita spending | $[X] | $[X] | $[X] |
| Property tax rate | [X] | [X] | [X] |

---

## Analysis Boundaries
<!-- Include for full budget reviews and fiscal impact analyses. Omit for quick highlights or single-item lookups. -->

*This fiscal analysis was produced by a single AI instance based on the documents provided.*

**Key assumptions that were not independently tested:**
- [Revenue projection assumption — e.g., "assumes X% growth based on 3-year trend"]
- [Cost estimate basis — e.g., "personnel costs estimated from budget document, not verified against actual payroll"]
- [Comparison methodology — e.g., "peer comparison uses published budgets, which may use different accounting treatments"]
- [State-law or tax-limit conclusion based on stale, missing, or incomplete state reference]

**Before relying on this analysis for budget decisions:**
- [ ] Finance director review of key figures and assumptions
- [ ] Verify any numbers marked Medium or Low confidence against source documents
- [ ] For significant fiscal policy changes, consider **PolicyAide multi-agent analysis** to stress-test assumptions through adversarial deliberation

---

*This analysis is for informational purposes.
Contact Finance Department for clarifications.*
```

## Skills Referenced

- `public-finance` — fund accounting, levy limits, debt analysis, fiscal impact methodology
- `policy-evaluation` — policy alignment analysis, evaluation criteria
- `municipal-code-analysis` — levy ordinance requirements, budget-related code provisions

## Notes

- Tailor depth to analysis type requested
- Always note assumptions and their basis
- Include confidence and provenance for key budget figures, fiscal estimates, state-law constraints, and peer comparisons
- Do not rely on stale or missing state references for levy limits, debt limits, budget deadlines, or state financial-data guidance
- Flag items that seem unusual or require explanation
- Compare to prior years for context
- Note one-time items separately
- Distinguish between controllable and non-controllable costs
- Identify structural balance vs. one-time fixes

## Related Skills

- `meeting-prep` — if the budget is on an upcoming meeting agenda
- `policy-research` — for research into policy alternatives with different fiscal profiles
- `intergovernmental-scan` — for state/federal funding changes affecting the budget
