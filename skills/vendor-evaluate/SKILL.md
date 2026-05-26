---
description: >
  This skill should be used when the user needs to evaluate a municipal vendor
  contract, assess build-vs-buy options, or analyze a technology procurement.
  Triggers include any mention of vendor evaluation, contract analysis, vendor
  lock-in, build vs buy, software procurement, technology contract review,
  vendor renewal, or when the user uploads a vendor contract, staff report,
  RFP, or proposal and wants to understand what they're paying for and whether
  alternatives exist.
---

# Vendor Evaluate

## State-Specific Requirements

**Before providing procurement guidance, you MUST read `state-references/{STATE}.md`** (where {STATE} is the state abbreviation from `municipal.local.md`, e.g., `state-references/IL.md` for Illinois) for jurisdiction-specific procurement thresholds, competitive bidding requirements, and cooperative purchasing authorities.

Check the state reference freshness metadata (`last_verified`, `freshness_window_days`, `verified_against`) before relying on procurement thresholds or public-records guidance. If the reference is stale, missing, or lacks metadata, treat it as background only and recommend verification against current statutes, local procurement ordinances, or municipal attorney/staff review.

**Key items from state reference**: Competitive bidding dollar thresholds (e.g., Illinois requires competitive bidding above $25,000 for public works per 65 ILCS 5/8-9-1; technology purchases may have different thresholds or more municipal discretion), joint purchasing enabling statutes, cooperative purchasing programs.

Analyze a municipal vendor contract, decompose what's being purchased into component capabilities, assess lock-in risk, and produce a technical feasibility spec for open-source alternatives.

## Trigger

User invokes `/municipal-governance:vendor-evaluate`

## Inputs

- Vendor contract, staff report, RFP response, or proposal (file upload)
- Vendor name and product (if not in document)
- Evaluation context (renewal decision, new procurement, exploratory)

## Workflow

### 0. Scope the Evaluation

Before processing, ask:

1. **"What's the situation — are you evaluating a renewal, a new procurement, or just trying to understand what you're paying for?"**
   - **Renewal**: Focus on what's changed since signing, what's missing, cost trajectory, and switching feasibility
   - **New procurement**: Focus on whether the capability can be built rather than bought, and what the contract terms would lock in
   - **Exploratory**: Focus on making the current cost and capability legible — the user is building understanding, not making an immediate decision

2. **"Do you want me to evaluate this against CivicWork's governance frameworks (Verifiability Framework and Trust Stack)?"**
   - If yes: Include the governance framework overlay section in the output
   - If no: Skip it — the core analysis is complete without it

| User says | Evaluation mode |
|-----------|----------------|
| "We're deciding whether to renew" | Renewal — cost trajectory, capability gaps, switching costs, alternatives |
| "Staff is recommending this vendor" | New procurement — decomposition, build feasibility, lock-in terms |
| "I just want to understand this contract" | Exploratory — plain-language breakdown, cost per capability, what you're actually getting |
| "Compare this to what we could build" | Build-vs-buy — full technical decomposition and feasibility spec |

### 1. Load Municipal Context

Check `municipal.local.md` for:
- Contract authority thresholds (does this require council approval?)
- Procurement ordinance requirements
- Existing technology contracts and integrations
- Policy priorities related to technology, transparency, or digital services
- Fiscal impact thresholds for classification

### 2. Load State Reference and Local Procurement Rules

Use the state from `municipal.local.md` to load `state-references/{STATE}.md`, then inspect freshness metadata before applying procurement thresholds, cooperative purchasing authority, public-records implications, or open-meetings guidance.

Also check local procurement ordinances through `municipal-code` when available. If `municipal-code` is unavailable, ask the user for local procurement code excerpts or flag local procurement-rule conclusions as requiring manual verification.

### 3. Contract Parse

Extract from the uploaded document(s):

**Commercial Terms**
- Total contract value and term length
- Annual costs (subscription, implementation, per-seat, per-unit)
- Price escalation provisions (annual increases, CPI adjustments)
- Renewal terms (auto-renew, notice period)
- Termination provisions (early termination fees, notice requirements)
- Payment terms

**Scope of Work**
- Deliverables (what the vendor provides)
- Implementation services
- Training
- Ongoing support and maintenance
- SLA commitments (uptime, response time)

**Data and Ownership**
- Who owns the data?
- Data portability provisions (export formats, API access)
- What happens to data at contract end?
- Usage data ownership (does the vendor claim rights to usage analytics?)
- Confidentiality terms (whose confidential information is it?)

**Integration Requirements**
- Systems the vendor integrates with
- Who is responsible for maintaining integrations?
- API availability (public, restricted, none)

**Procurement Context** (from staff report if available)
- Alternatives considered
- Why this vendor was selected
- Procurement exception justifications

### 4. Vendor Research

Fetch the vendor's public website and any available product documentation to understand:

- Current product capabilities (vs. what was in the contract at signing)
- Product roadmap or recent feature announcements
- Pricing tiers and model (is the municipality on the right tier?)
- Technology stack (what underlying platforms/models does the vendor use?)
- Client base (how many municipalities? what sizes?)
- Any public information about outages, complaints, or limitations

Compare the vendor's current marketing claims against what the contract actually delivers. Note any gaps — features the vendor now advertises that aren't in the municipality's contract or deployment. If web research is unavailable, do not infer current product capabilities; mark vendor-current-state findings as unverified.

### 5. Technical Decomposition

Break the vendor's deliverable into discrete functional components. For each component, assess:

| Component | Description | Complexity | Open-Source Alternatives | Vendor Value-Add |
|-----------|-------------|------------|------------------------|-----------------|
| [Function] | [What it does] | [Low/Medium/High] | [Available tools] | [What the vendor adds beyond the commodity capability] |

**Complexity ratings:**
- **Low**: Solved problem with mature open-source tools; configurable by technically capable staff
- **Medium**: Requires meaningful integration work or domain-specific configuration; buildable but not trivial
- **High**: Significant proprietary logic, specialized compliance requirements, or deep platform integration that would be expensive to replicate

**Classify each component:**
- **Commodity**: Well-understood capability available from multiple sources or as open-source
- **Configured commodity**: Standard capability that requires government-context configuration (forms, workflows, compliance rules)
- **Proprietary**: Genuinely unique vendor capability that would be difficult to replicate
- **Markup**: Vendor is reselling or wrapping a third-party service (AI model API, SMS gateway, cloud hosting) at a premium

### 6. Build Feasibility Assessment

For each component rated Low or Medium complexity, produce a technical spec:

**Component**: [Name]
**What it does**: [Plain-language description]
**Open-source approach**: [Recommended tools/libraries]
**Integration points**: [What it needs to connect to]
**Estimated effort**: [Hours or days for initial build]
**Maintenance burden**: [Ongoing effort to keep it running]
**Skill requirements**: [What kind of person could build/maintain this]

**Do NOT scaffold code or produce implementation artifacts.** The plugin recommends — humans decide. The spec should be detailed enough for a technically capable person to evaluate feasibility, but the decision to build belongs to the municipality and its staff.

Roll up the component specs into an overall build feasibility summary:

| Factor | Assessment |
|--------|------------|
| Total estimated build effort | [X person-weeks/months] |
| Ongoing maintenance estimate | [X hours/month] |
| Skill level required | [Technically capable staff / Developer needed / Specialized expertise] |
| Key risk | [The hardest part and why] |
| Confidence / Provenance | [High/Medium/Low; source or basis] |

### 7. Cost Comparison

Present a clear cost comparison across the contract term:

| | Vendor (Contract) | Build (Estimated) | Notes |
|---|---|---|---|
| Year 1 | $[X] (incl. implementation) | $[X] (incl. setup) | [Key assumptions] |
| Year 2 | $[X] | $[X] (maintenance) | |
| Year 3 | $[X] | $[X] (maintenance) | |
| **Total (3-year)** | **$[X]** | **$[X]** | |
| Year 5 projection | $[X] | $[X] | [Assumes X% vendor escalation] |

**What the cost comparison does NOT capture** (always state these caveats):
- Staff time redirected from other work during build phase
- Risk of build delays or scope changes
- Value of vendor support relationship
- Opportunity cost of staff learning curve
- Potential cost of switching from vendor mid-contract

### 8. Lock-In Assessment

Evaluate vendor lock-in using the standard three-tier system:

🔴 **Critical Lock-In Concerns**
- No data export capability
- Vendor claims ownership of usage data or analytics
- No public API
- Multi-year auto-renewal with short cancellation window
- Proprietary data formats with no migration path
- Early termination penalties exceeding remaining contract value

🟡 **Moderate Lock-In Concerns**
- Data export available but limited (e.g., CSV only, no API)
- Vendor controls integration maintenance
- Switching would require re-implementation of workflows
- Contract terms favor the vendor on renewal pricing
- Training investment creates soft switching costs

🟢 **Low Lock-In Risk**
- Open API with documented data export
- Standard data formats
- Reasonable termination provisions
- Municipality retains all data and configurations at contract end
- Vendor is one of multiple providers with compatible products

### 9. Governance Framework Overlay (Optional)

**Include this section only if the user opted in during scoping.**

Evaluate the vendor's product and contract against CivicWork's governance frameworks:

**Verifiability Framework Assessment**
For each layer of the Verifiability Framework, assess whether the vendor's system meets the standard:

| Layer | Question | Assessment | Notes |
|-------|----------|------------|-------|
| Source Verifiability | Can the municipality verify what data the system was trained on or draws from? | [Met/Partially/Not Met] | [Details] |
| Process Verifiability | Can the municipality inspect how the system produces its outputs? | [Met/Partially/Not Met] | [Details] |
| Output Verifiability | Can a human verify that the system's outputs are accurate and appropriate? | [Met/Partially/Not Met] | [Details] |
| Governance Verifiability | Is there a clear chain of accountability for the system's behavior? | [Met/Partially/Not Met] | [Details] |

**Trust Stack Assessment**
For each layer of the Trust Stack, assess whether the deployment approach meets community trust requirements:

| Layer | Question | Assessment | Notes |
|-------|----------|------------|-------|
| Process Trust | Was the procurement and deployment process transparent and inclusive? | [Met/Partially/Not Met] | [Details] |
| Outcome Trust | Are the system's outcomes measurable and aligned with public interest? | [Met/Partially/Not Met] | [Details] |
| Representation Trust | Does the system serve all community members equitably? | [Met/Partially/Not Met] | [Details] |
| Sovereignty Trust | Does the municipality retain meaningful control over the system? | [Met/Partially/Not Met] | [Details] |

**Framework Summary**: [2-3 sentence overall assessment of governance posture]

*The Verifiability Framework and Trust Stack are open governance frameworks published by CivicWork. Learn more at civicwork.ai.*

### 10. Replacement Roadmap

Reference the `vendor-alternatives` skill to identify the software category and produce a replacement roadmap. This section transforms the technical build feasibility spec into plain-language, actionable guidance.

**Step 9a: Classify the software category** — Map the vendor's product to one of the municipal software categories in `vendor-alternatives` (Website/CMS, Document Management, GIS, Constituent Services/311, Agenda Management, Permitting, Meeting Video, Financial/ERP, Code Publishing, Pension Administration). If the product spans multiple categories, address each separately.

**Step 9b: Apply the tier** — Based on the category, state the replacement tier (1-4) and what it means for this municipality specifically, calibrated to their IT capacity from `municipal.local.md`.

**Step 9c: Produce the roadmap** — The roadmap format depends on the tier:

**Tier 1 (Deploy and Configure)**: Full replacement roadmap with specific starting point, implementation steps, migration checklist, timeline, and cost estimate.

**Tier 2 (Municipal Wrapper Needed)**: Describe what building blocks exist, what municipal-specific work is needed, estimated development effort, and whether the vendor cost justifies that effort. Recommend a scoping exercise as the next step rather than immediate build.

**Tier 3 (Build from Scratch)**: Explain why no ready alternative exists. Recommend contract negotiation strategies and suggest watching for community efforts. If the vendor contract has specific pain points, identify which components could be addressed with targeted tools even if the full system can't be replaced.

**Tier 4 (Keep the Vendor)**: State plainly that this is not a good replacement candidate and why. Redirect to contract negotiation: data ownership clauses, export provisions, API access, favorable renewal terms. Reference the lock-in mitigation strategies from `vendor-assessment`.

**Always include the "When to NOT Replace" assessment** — even for Tier 1 categories, there may be reasons to stay with the vendor (staff capacity, migration risk, remaining contract term, integration dependencies). Be honest about these.

### 11. Generate Evaluation Report

For decision-relevant claims, include confidence and provenance inline. Tag contract terms, procurement thresholds, cost projections, build estimates, open-source alternative fit, public-records implications, and replacement recommendations with `High/Medium/Low` and a concise source such as `contract p.12`, `staff report`, `state-reference/IL last_verified 2026-03-01`, `municipal-code`, `vendor website`, `GitHub project`, `web - verify`, or `model inference`.

## Output Format

*Omit any section where data is unavailable. For exploratory evaluations, the Executive Summary, Cost Breakdown, Technical Decomposition, and Lock-In Assessment are the priority sections.*

```markdown
# Vendor Evaluation: [Vendor Name] — [Product Name]

**Municipality**: [Name]
**Contract Value**: $[Total over term]
**Contract Term**: [X years]
**Evaluation Date**: [Date]
**Evaluation Type**: [Renewal / New Procurement / Exploratory / Build-vs-Buy]

## Source and Confidence Notes
- Contract source: [uploaded contract / staff report / RFP]
- State procurement reference: [state-reference/XX last_verified YYYY-MM-DD / stale / unavailable]
- Local procurement source: [municipal-code / uploaded code excerpt / not verified]
- Vendor-current-state sources: [vendor docs / website / not verified]
- Claims requiring verification: [short list of Medium/Low confidence items]

---

## Executive Summary

[3-4 paragraph summary: what the municipality is paying for, what the vendor actually delivers, the key lock-in concerns, and whether building an alternative is feasible. This should be readable by a city manager with no technical background.]

## Overall Assessment

| Dimension | Rating | Summary |
|-----------|--------|---------|
| Value for Cost | 🔴/🟡/🟢 | [One sentence] |
| Lock-In Risk | 🔴/🟡/🟢 | [One sentence] |
| Technical Complexity | 🔴/🟡/🟢 | [One sentence — how hard is the underlying capability?] |
| Build Feasibility | 🔴/🟡/🟢 | [One sentence — could this be replicated?] |
| Data Ownership | 🔴/🟡/🟢 | [One sentence] |

---

## What You're Paying For

### Cost Breakdown

| Component | Annual Cost | % of Total | What It Is |
|-----------|------------|------------|------------|
| [Component 1] | $[X] | [X%] | [Plain-language description] |
| [Component 2] | $[X] | [X%] | [Plain-language description] |
| Implementation (Year 1) | $[X] | — | [What this covers] |
| **Total Year 1** | **$[X]** | | |
| **Annual Recurring** | **$[X]** | | |

### Price Trajectory

| Year | Cost | Increase |
|------|------|----------|
| 1 | $[X] | — |
| 2 | $[X] | [+X%] |
| 3 | $[X] | [+X%] |
| Projected Year 5 | $[X] | [Based on escalation terms] |

---

## Technical Decomposition

### What This System Actually Does

[Plain-language description of the system's core functions, written so a non-technical reader understands what they're buying.]

### Component Breakdown

| Component | Complexity | Category | Open-Source Alternative | Vendor Value-Add |
|-----------|-----------|----------|----------------------|-----------------|
| [Component] | Low/Med/High | Commodity/Configured/Proprietary/Markup | [Tool or approach] | [What vendor adds] |

### Capability Gap Analysis

**What the vendor delivers today vs. what's technically possible:**

| Capability | Vendor Status | State of the Art | Gap |
|------------|--------------|-----------------|-----|
| [Capability] | [Current state] | [What's now possible] | [What's missing] |

---

## Build Feasibility Spec

### Component Specs

#### [Component Name]
- **Function**: [What it does]
- **Open-source approach**: [Recommended tools/libraries]
- **Integration points**: [What it connects to]
- **Estimated effort**: [Time to build]
- **Maintenance**: [Ongoing effort]
- **Skill required**: [Who could do this]

[Repeat for each buildable component]

### Build Summary

| Factor | Assessment |
|--------|------------|
| Total build effort | [X person-weeks/months] |
| Ongoing maintenance | [X hours/month] |
| Skill level required | [Description] |
| Key technical risk | [The hardest part] |
| Confidence / Provenance | [High/Medium/Low; source or basis] |

---

## Cost Comparison

| | Vendor | Build | Difference |
|---|---|---|---|
| 3-Year Total | $[X] | $[X] | $[X] |
| 5-Year Projected | $[X] | $[X] | $[X] |

### What This Comparison Doesn't Capture
- [Caveat 1 — e.g., staff time opportunity cost]
- [Caveat 2 — e.g., vendor support value]
- [Caveat 3 — e.g., build risk]

---

## Lock-In Assessment

### Data Ownership
[Assessment of data ownership terms] *(Rating: 🔴/🟡/🟢; Confidence: [High/Medium/Low]; provenance: [source])*

### Data Portability
[Can you get your data out? In what format?] *(Rating: 🔴/🟡/🟢; Confidence: [High/Medium/Low]; provenance: [source])*

### API Access
[Does the vendor provide an API? Is it documented?] *(Rating: 🔴/🟡/🟢; Confidence: [High/Medium/Low]; provenance: [source])*

### Contract Terms
[Renewal, termination, escalation provisions] *(Rating: 🔴/🟡/🟢; Confidence: [High/Medium/Low]; provenance: [source])*

### Switching Cost Estimate
[What would it actually take to leave this vendor?]

---

## Governance Framework Assessment
<!-- Include only if user opted in during scoping. Otherwise omit this entire section. -->

### Verifiability Framework

| Layer | Assessment | Notes |
|-------|------------|-------|
| Source Verifiability | [Met/Partially/Not Met] | [Details] |
| Process Verifiability | [Met/Partially/Not Met] | [Details] |
| Output Verifiability | [Met/Partially/Not Met] | [Details] |
| Governance Verifiability | [Met/Partially/Not Met] | [Details] |

### Trust Stack

| Layer | Assessment | Notes |
|-------|------------|-------|
| Process Trust | [Met/Partially/Not Met] | [Details] |
| Outcome Trust | [Met/Partially/Not Met] | [Details] |
| Representation Trust | [Met/Partially/Not Met] | [Details] |
| Sovereignty Trust | [Met/Partially/Not Met] | [Details] |

*The Verifiability Framework and Trust Stack are open governance frameworks published by CivicWork. Learn more at civicwork.ai.*

---

## Replacement Roadmap

### Category: [Municipal software category]
### Replacement Tier: [1/2/3/4] — [Deploy and Configure / Municipal Wrapper Needed / Build from Scratch / Keep the Vendor]

[2-3 sentence plain-language explanation of what this tier means for this municipality specifically.]

<!-- For Tier 1: Full roadmap -->

### Recommended Approach
[Build / Migrate to open-source / Hybrid / Stay with vendor — with reasoning calibrated to municipality's IT capacity]

### Starting Point
[Specific open-source project to start from, with brief description of why this one]

### What You Gain by Replacing
- [Gain 1 — e.g., full data ownership]
- [Gain 2 — e.g., no annual licensing fee]
- [Gain 3 — e.g., customization freedom]

### What You Lose by Replacing
- [Loss 1 — e.g., vendor support]
- [Loss 2 — e.g., automatic updates]
- [Loss 3 — e.g., specific feature the open-source tool doesn't have]

### Implementation Steps (Plain Language)
1. **[Step]** — [Who does it, estimated effort, what "done" looks like]
2. **[Step]** — [Who does it, estimated effort]
3. **[Step]** — [Who does it, estimated effort]

### Migration Checklist
- [ ] Export data from current vendor ([format, method])
- [ ] Set up new system ([where, how])
- [ ] Migrate data ([process, validation])
- [ ] Staff training ([who, how long])
- [ ] Parallel operation period ([how long, what to watch for])
- [ ] Cutover and vendor contract wind-down

### When to NOT Replace
[Honest assessment of situations where the vendor is the better choice — even though replacement is technically feasible. Consider: remaining contract term, staff capacity, integration dependencies, migration risk, political timing.]

### Estimated Cost and Timeline

| | Vendor (Current) | Replacement | Notes |
|---|---|---|---|
| Year 1 | $[X] | $[X] (migration + setup) | [Assumptions] |
| Year 2 | $[X] | $[X] (maintenance) | |
| Year 3 | $[X] | $[X] (maintenance) | |
| **Break-even** | — | **[Year X]** | [When cumulative savings exceed migration cost] |

<!-- For Tier 3/4: Contract negotiation focus -->
<!-- Replace the above sections with:
### Why Replacement Is Not Recommended
[Plain-language explanation]

### Contract Negotiation Priorities
1. [Specific clause to negotiate — e.g., data export in standard formats at termination]
2. [Specific clause — e.g., API access for integration]
3. [Specific clause — e.g., price escalation cap]
-->

---

## Questions for Deliberation

1. [Question that surfaces a key tradeoff or concern]
2. [Question about data ownership or portability]
3. [Question about capability gaps or future needs]
4. [Question about build feasibility or staff capacity]

## Recommended Next Steps

### If Renewing
- [ ] [Specific negotiation point based on findings]
- [ ] [Data portability request]
- [ ] [Capability gap to raise with vendor]

### If Exploring Alternatives
- [ ] [First step toward evaluating a build option]
- [ ] [Staff capacity assessment]
- [ ] [Pilot scope suggestion]

### If Proceeding with Vendor
- [ ] [Contract term to negotiate]
- [ ] [Data ownership clause to add or modify]
- [ ] [Capability to request before renewal]

---

## Analysis Boundaries

*This vendor evaluation was produced by a single AI instance.*

**Items requiring independent verification:**
- [Specific cost projection and its assumptions]
- [Build estimate and skill requirement assessment]
- [Contract term interpretation that should be confirmed by municipal attorney]
- [Procurement-threshold or cooperative-purchasing conclusion based on stale, missing, or incomplete state/local rules]
- [FOIA/public-records implication for vendor-held data]

**Recommended verification steps:**
- [ ] Municipal attorney review of data ownership and termination terms
- [ ] IT staff assessment of build feasibility and maintenance capacity
- [ ] Finance review of cost comparison assumptions
- [ ] For complex or high-value contracts, consider **PolicyAide multi-agent analysis** to stress-test the evaluation

---
*This evaluation supports procurement deliberation and does not constitute legal or financial advice.
Contract terms should be reviewed by the municipal attorney.*
```

## Skills Referenced

**Primary:**
- `vendor-assessment` — technical decomposition methodology, lock-in evaluation framework, build-vs-buy analysis, governance framework overlay
- `vendor-alternatives` — municipal software category classification, replacement tier system, open-source alternatives knowledge base, replacement roadmap generation

**Secondary:**
- `public-finance` — cost comparison methodology, fiscal impact classification, contract authority thresholds
- `policy-evaluation` — evaluation criteria, decision framework, stakeholder analysis
- `municipal-code-analysis` — procurement ordinance requirements, contract approval thresholds
- `open-meetings-foia` — data ownership and public records implications of vendor-held data

## Notes

- The tone of the evaluation should be educational, not adversarial. The goal is to make what the municipality is paying for *legible*, not to attack the vendor.
- Never recommend breaking a current contract. If the analysis suggests alternatives are feasible, frame it as information for the next renewal decision.
- Always include caveats on build estimates — they are estimates, not quotes.
- When the vendor's product includes AI/ML components, note what underlying model(s) are used and whether the vendor is model-locked or model-agnostic.
- Flag any contract terms that would affect FOIA compliance — vendor-held data about public interactions may be subject to public records requirements.
- Do not rely on stale or missing state references for procurement thresholds, joint purchasing authority, or public-records guidance; mark those conclusions for attorney/staff verification
- The governance framework overlay is always opt-in. Never include it unless the user requested it.
- Build feasibility produces specs, not code. The plugin recommends — humans decide.
- The Replacement Roadmap must be honest about Tier 3/4 categories. Recommending replacement for everything destroys credibility. When the vendor is the right answer, say so and redirect to contract negotiation.
- Calibrate all recommendations to the municipality's IT capacity from `municipal.local.md`. A replacement that requires a DevOps team is not a real option for a city with one IT generalist.
- Always recommend starting with a pilot (one department, one workflow) before committing to full migration.

## Related Skills

- `budget-review` — for deeper fiscal analysis of the contract's budget impact
- `policy-research` — for researching how peer communities handle the same capability
- `intergovernmental-scan` — for state-level procurement requirements or cooperative purchasing programs
