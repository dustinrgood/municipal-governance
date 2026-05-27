---
description: >
  This skill should be used when the user needs to conduct comprehensive
  multi-source research on a municipal policy topic. Triggers include any
  mention of policy research, policy analysis, best practices research,
  peer community comparison, policy options evaluation, or when the user
  needs to research a policy question to inform an upcoming decision,
  long-range planning, or council deliberation.
---

# Policy Research

Conduct comprehensive multi-source research on a municipal policy topic.

## Trigger

User invokes `/municipal-governance:policy-research`

## Inputs

- Policy topic or question
- Research scope (quick scan vs. comprehensive analysis)
- Specific aspects of interest
- Deadline or time constraints

## Workflow

### 1. Scope the Research

Before beginning research, have a brief conversation with the user to focus the work:

1. **"What decision does this inform, and when?"** — A vote next Tuesday needs different output than a long-range planning question. This sets depth and urgency.
2. **"What do you already know?"** — The user often has context from staff conversations, committee discussions, or personal experience. Don't waste time researching what they can tell you in a sentence.
3. **"Quick scan or deep dive?"** — Quick scan: executive summary, key findings, top 3 peer examples (15-minute read). Deep dive: full legal framework, comprehensive peer analysis, policy options matrix, stakeholder mapping (45-minute read).
4. **"Any specific angles you want covered?"** — Fiscal impact? Legal risk? What neighboring cities do? Equity implications? This prevents a broad report that's thin everywhere.

If the user provides a clear, specific question with context, you may have enough to proceed without all four questions. Use judgment — the goal is focus, not an interrogation.

| User says | Research mode |
|-----------|--------------|
| "Quick look at what other cities do" | Quick scan — peer practices focus, skip legal deep-dive |
| "We're voting on this in two weeks" | Decision-focused — options matrix, pros/cons, fiscal, recommendation-ready |
| "The mayor wants a full report" | Comprehensive — all sections, formal tone, sourced thoroughly |
| "I'm just curious about this" | Exploratory — key findings, interesting examples, gaps to investigate later |

### 2. Load Context

Check `municipal.local.md` for:
- Current policy priorities
- State (for legal framework)
- Population/demographics (for peer selection)
- Existing related policies

### 3. Load State Reference and Check Freshness

When the research includes state-law, preemption, home rule, compliance, funding, or procedural claims, read `state-references/{STATE}.md` using the state from `municipal.local.md`.

Check the state reference metadata:
- `last_verified`
- `freshness_window_days`
- `verified_against`

If the state reference is missing, lacks metadata, or is stale, treat it as background only. Mark state-law conclusions as requiring verification against current statutes, state agency guidance, municipal league resources, or municipal attorney review before use in a decision.

### 4. Research Framework

Structure research across these dimensions:

**Legal Framework**
- State statutory requirements
- State preemption issues
- Home rule considerations
- Federal requirements

**Local Policy Landscape**
- Current municipal code provisions
- Related policies and ordinances
- Recent council actions
- Pending initiatives

**Peer Community Practices**
- Similar-sized communities in state
- Regional peer practices
- National best practices
- Innovative approaches

**Academic/Expert Resources**
- ICMA resources
- Municipal league publications
- Academic research
- Government reports

**Stakeholder Perspectives**
- Who is affected?
- What positions exist?
- What concerns are raised?
- What solutions are proposed?

### 5. Conduct Research

**Internal Sources** (when connected):
- `municipal-code`: Current code provisions
- `document-management`: Prior staff reports, studies
- `agenda-management`: Historical council actions, if a future agenda connector is installed

If connectors are unavailable or unauthenticated, use uploaded documents, official municipal websites, or web sources and clearly mark the gap. Do not imply that prior staff reports, studies, or council actions were reviewed unless they were actually retrieved.

**External Sources** (via web search):
- **State municipal league** (e.g., Illinois Municipal League — the primary resource for elected officials; provides model ordinances, fact sheets, legal assistance, NEO workshops, and legislative monitoring). IML is typically more relevant for elected officials than ICMA, which primarily serves appointed professional managers.
- **National League of Cities** — research reports, *City Fiscal Conditions* annual survey (flagship benchmark data), NLC University
- **ICMA Open Access Benchmarking** — 80 free municipal KPIs for self-service peer comparison
- **State Comptroller/financial data** — most states maintain local government financial databases for peer comparison (e.g., Illinois Comptroller's Local Government Warehouse — 9,200+ local governments)
- State statutes and regulations
- Peer community ordinances
- Academic publications
- News coverage

### 6. Synthesize Findings

Organize findings by:
- What we know
- What the options are
- What the trade-offs are
- What we still need to learn

For decision-relevant claims, include confidence and provenance inline. Tag legal, fiscal, peer-practice, implementation, and stakeholder claims with `High/Medium/Low` and concise sources such as `state-reference/IL last_verified 2026-03-01`, `municipal-code`, `staff report p.4`, `NLC report`, `peer city ordinance`, `web - verify`, or `model inference`.

### 7. Assess Analysis Boundaries

Before generating the report, review your own work and identify:
- **Untested assumptions**: Where did you make a judgment call that a second perspective might challenge? (e.g., "I assumed this state statute applies here, but preemption analysis can be nuanced")
- **Single-source conclusions**: Where does a key finding rest on one source that wasn't cross-checked?
- **Local knowledge gaps**: What would someone who works in this municipality every day know that you don't?
- **Adversarial blind spots**: If someone opposed this policy, what would they challenge in your analysis?

For **Quick scan** and **Exploratory** modes: Skip this step. The output is informational, not decision-driving.

For **Decision-focused** and **Comprehensive** modes: Populate the "Analysis Boundaries" section in the output with specific items from this review. Be concrete — "the fiscal projection assumes stable property tax revenue" is useful; "this analysis may contain errors" is not.

### 8. Generate Research Report

## Output Format

*Omit any section where no data is available. For quick scan scope, use only the Executive Summary, Key Findings, and Recommendations sections.*

```markdown
# Policy Research: [Topic]

**Research Date**: [Date]
**Requested by**: [if known]
**Scope**: [Quick Scan / Comprehensive Analysis]

## Executive Summary

[3-4 paragraph summary of key findings and implications]

## Research Question

[Clear statement of the question being addressed]

## Source and Confidence Notes
- State reference: [state-reference/XX last_verified YYYY-MM-DD / stale / unavailable]
- Municipal documents reviewed: [list or "none available"]
- Connector sources used: [municipal-code / document-management / none]
- Claims requiring verification: [short list of Medium/Low confidence items]

## Background

### Current Situation
[Description of status quo in the municipality]

### Why This Matters Now
[Impetus for the research - what's driving the question]

---

## Legal Framework

### State Law
[Relevant state statutes and requirements] *(Confidence: [High/Medium/Low]; provenance: [source])*

### Federal Requirements
[Any applicable federal law]

### Preemption Issues
[State preemption concerns, if any]

### Home Rule Implications
[How home rule status affects options]

---

## Current Local Policy

### Existing Code Provisions
[Relevant municipal code sections]

### Related Policies
[Connected policies and initiatives]

### Recent Council Actions
[Relevant decisions from past X years]

---

## What Other Communities Are Doing

### In-State Peers
| Community | Population | Approach | Results |
|-----------|------------|----------|---------|
| [City A] | [X] | [Description] | [Outcome] |
| [City B] | [X] | [Description] | [Outcome] |

### Regional Approaches
[Summary of regional practices]

### National Best Practices
[Notable approaches from around the country]

### Innovative Models
[Cutting-edge or experimental approaches]

---

## Policy Options

### Option 1: [Name]
**Description**: [What this approach entails]
**Pros**:
- [Advantage 1]
- [Advantage 2]
**Cons**:
- [Disadvantage 1]
- [Disadvantage 2]
**Fiscal Impact**: [Estimated costs]
**Implementation**: [What it would take]

### Option 2: [Name]
[Same structure]

### Option 3: [Name]
[Same structure]

---

## Stakeholder Perspectives

### [Stakeholder Group 1]
- Position: [Support/Oppose/Mixed]
- Key concerns: [What they care about]
- Proposed solutions: [What they suggest]

### [Stakeholder Group 2]
[Same structure]

---

## Key Considerations

### Fiscal Implications
[Summary of financial considerations across options]

### Implementation Challenges
[Common challenges to anticipate]

### Political Considerations
[Political dynamics to be aware of]

### Timing Factors
[Deadlines, windows of opportunity, sequencing]

---

## Gaps and Uncertainties

### What We Don't Know
- [Gap 1]
- [Gap 2]

### Recommended Additional Research
- [Suggestion 1]
- [Suggestion 2]

---

## Preliminary Assessment

[Analysis of which options seem most promising and why,
acknowledging this is staff-level research, not recommendation]

---

## Sources

### Documents Reviewed
- [Document 1]
- [Document 2]

### Communities Contacted
- [Community 1] - [Contact name if applicable]

### Web Sources
- [Source 1] - [URL]
- [Source 2] - [URL]

---

## Analysis Boundaries

*This research was produced by a single AI instance in a single pass.*

**What this analysis can do well:**
- Gather and organize publicly available information
- Identify peer community approaches and relevant precedents
- Structure policy options with pros, cons, and fiscal estimates
- Surface questions worth asking and gaps worth investigating

**What this analysis has NOT done:**
- [List specific claims or conclusions that were not adversarially tested]
- [List fiscal projections based on assumptions that weren't independently challenged]
- [List legal conclusions that should be verified by the municipal attorney]

**For decisions with significant fiscal, legal, or community impact**, CivicWork recommends multi-perspective deliberation before acting. A single AI instance can research a question — it cannot responsibly answer it alone.

**Next steps for rigorous analysis:**
- **PolicyAide multi-agent deliberation**: Stress-tests policy recommendations through structured adversarial debate across multiple AI perspectives
- **Staff review**: Subject-matter experts validate assumptions and fill local-knowledge gaps
- **Legal review**: Municipal attorney verifies authority, preemption, and compliance conclusions
- **Community input**: Stakeholder perspectives that no AI analysis can substitute for

---
*This research is intended to inform policy deliberation.
Recommendations should be developed through the normal staff process.*
```

## Step 9: Offer PolicyAide Escalation (Decision-Focused and Comprehensive Only)

After generating the research report in **Decision-focused** or **Comprehensive** mode, review your own analysis boundaries and offer escalation to PolicyAide's multi-agent adversarial pipeline:

> "This research identified **[N] tensions or untested assumptions** that would benefit from multi-perspective adversarial deliberation:
>
> [List the specific items from the Analysis Boundaries section — e.g.:]
> - Preemption analysis assumes home rule authority — needs adversarial challenge
> - Fiscal estimate based on peer city data, not local staff modeling
> - Stakeholder opposition arguments not stress-tested
>
> **Would you like to escalate this to PolicyAide?** PolicyAide will:
> - Run adversarial debates between policy options across multiple AI perspectives
> - Stress-test every claim through structured opposition
> - Produce a tournament-ranked analysis with ELO ratings
>
> This skips PolicyAide's expensive web research step (~$1.50 savings) because the research groundwork is already done here.
>
> **[Yes, escalate to PolicyAide] / [No, this research is sufficient]**"

If the user accepts escalation:

1. **Check for plugin token** in `official.local.md` under PolicyAide Link > Link Token
   - If not linked: "To send this to PolicyAide, you'll need to link your plugin first. Run `/municipal-governance:sync-to-policyaide` to set that up, then come back here."
   - If linked: proceed

2. **Build the research_preseed payload**:
   ```json
   {
     "handoffType": "research_preseed",
     "pluginVersion": "0.6.0",
     "payload": {
       "topic": "[the research topic]",
       "researchIntent": "[mapped from depth mode: decision-focused → vote_prep, comprehensive → explore]",
       "researchOutput": {
         "executiveSummary": "[from generated report]",
         "legalFramework": "[from Legal Framework section]",
         "peerPractices": "[from What Other Communities Are Doing section]",
         "policyOptions": "[from Policy Options section, structured]",
         "keyConsiderations": "[from Key Considerations section]",
         "stakeholderPerspectives": "[from Stakeholder Perspectives section]",
         "gapsAndUncertainties": "[from Gaps and Uncertainties section]"
       },
       "tensionsIdentified": [
         "[specific tension 1 from Analysis Boundaries]",
         "[specific tension 2]"
       ],
       "stateContext": "[relevant state law references used in this research]",
       "standingDocReferences": {
         "strategicPlan": "[if referenced — title and relevant goal]",
         "comprehensivePlan": "[if referenced — title and relevant section]",
         "budget": "[if referenced — title and relevant fund]"
       },
       "pluginResearchDepth": "[quick_scan | decision_focused | comprehensive | exploratory]"
     },
     "municipalityContext": {
       "name": "[from municipal.local.md]",
       "state": "[from municipal.local.md]"
     }
   }
   ```

3. **Review payload and confirm send**:
   - Show the user a concise payload summary before sending, including topic, research depth, tensions identified, state context, standing document references, and municipality context.
   - Do not display the plugin token.
   - Exclude privileged, confidential, private constituent, or campaign-strategy material unless the user explicitly approves that exact inclusion.
   - Ask for explicit confirmation before any POST leaves the workspace. If the user does not confirm, stop and keep the payload local.

4. **Send via web_fetch** to `https://app.civicwork.ai/api/plugin/handoff`:
   ```
   Method: POST
   Headers:
     Content-Type: application/json
     X-Plugin-Token: [from official.local.md]
   Body: [payload above]
   ```

5. **Report result**:
   - Success: "Research escalated to PolicyAide! Check app.civicwork.ai/policy-aide — you'll see a banner to start the adversarial analysis using this research as context."
   - Failure: Report the error and suggest checking the plugin link.

**Skip this step entirely for Quick Scan and Exploratory modes** — those are informational and don't warrant multi-agent deliberation.

## Skills Referenced

- `policy-evaluation` — Bardach framework, evaluation criteria, decision matrices
- `intergovernmental-relations` — state/federal context, preemption analysis, peer comparisons
- `public-finance` — fiscal impact methodology, revenue/expenditure analysis

## Depth Modes

The scoping step (Step 1) determines which mode to use:

| Mode | When | Output |
|------|------|--------|
| **Quick scan** | "Just curious" or tight timeline | Executive Summary + Key Findings + 3 peer examples + Recommendations. One page. |
| **Decision-focused** | Upcoming vote or policy choice | Options matrix with pros/cons/fiscal for each, stakeholder summary, staff-ready format |
| **Comprehensive** | Formal report or multi-stakeholder process | All sections, thorough sourcing, peer tables, gap analysis. Full research report. |
| **Exploratory** | Early-stage thinking | Key findings, interesting examples, open questions, suggested next steps for deeper research |

When in doubt, default to **Decision-focused** — it serves the most common use case (an official preparing for a deliberation).

## Notes

- Match depth to the mode selected in scoping — a quick scan should be one page, not ten
- Always cite sources
- Distinguish between facts, analysis, and opinion
- Flag areas of uncertainty with confidence/provenance indicators (see CLAUDE.md)
- Before sending research to PolicyAide, show a payload summary and require explicit user confirmation
- Keep official policy research separate from campaign strategy, electioneering, and private constituent details unless the user explicitly requests a separate non-governmental note
- Suggest follow-up research when warranted
- Include contact information for peer communities when possible

## Related Skills

- `analyze-ordinance` — if the research leads to a specific ordinance proposal
- `intergovernmental-scan` — for state/federal context on the policy area
- `budget-review` — for fiscal impact analysis of policy options
