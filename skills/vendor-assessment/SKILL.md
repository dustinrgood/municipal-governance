---
description: >
  This skill should be used when the user needs to understand vendor
  relationships, technology procurement decisions, build-vs-buy tradeoffs,
  vendor lock-in, data ownership, or software cost structures in a municipal
  context. Triggers include any mention of vendor lock-in, data portability,
  build vs buy, technology procurement, contract evaluation, SaaS costs,
  open-source alternatives, or when assessing whether a municipality should
  own vs rent its software infrastructure.
---

# Vendor Assessment

## State-Specific Requirements

**Before providing procurement guidance, you MUST read `state-references/{STATE}.md`** (where {STATE} is the state abbreviation from `municipal.local.md`, e.g., `state-references/IL.md` for Illinois) for jurisdiction-specific procurement thresholds, competitive bidding requirements, and cooperative purchasing authorities.

Check the state reference freshness metadata (`last_verified`, `freshness_window_days`, `verified_against`) before relying on procurement thresholds, competitive bidding requirements, joint purchasing authority, or public-records implications. If the reference is missing, stale, or lacks metadata, treat it as background only and recommend verification against current statutes, local procurement ordinances, staff, or municipal attorney review.

**Key items from state reference**: Competitive bidding dollar thresholds, joint purchasing enabling statutes, cooperative purchasing programs. For small municipalities (~15,000 population), cooperative purchasing through state contracts and joint purchasing programs is typically the most cost-effective procurement strategy.

## Overview

Municipalities spend significant portions of their budgets on software and technology services, often through multi-year SaaS contracts with specialized govtech vendors. These relationships can deliver real value — but they can also create dependencies that erode municipal control over public data, inflate costs over time, and limit the organization's ability to adapt to rapidly changing technology.

This skill provides frameworks for evaluating vendor relationships, understanding what municipalities are actually paying for, and assessing whether alternatives — including open-source and in-house approaches — are feasible.

## The Municipal Software Landscape

### How Municipalities Acquire Software

Most municipal software follows one of three procurement paths:

**Enterprise SaaS contracts**: Annual subscriptions for cloud-hosted platforms. The vendor provides the software, hosting, maintenance, and support. The municipality configures the platform for its needs. Common for ERP, CRM, permitting, FOIA management, agenda management, and utility billing.

**Custom development**: A software consultant builds a bespoke application for the municipality. The city may own the code, or the consultant may retain IP rights. Common for citizen portals, internal tools, and integrations between systems.

**Cooperative purchasing**: The municipality buys through a state master contract or cooperative purchasing agreement (e.g., NASPO, U.S. Communities). Reduces procurement overhead but limits negotiation on terms.

### The Consolidation Pattern

The govtech vendor landscape has consolidated rapidly. A small number of platform companies — Granicus, CivicPlus, Tyler Technologies, Accela — have acquired dozens of niche products and bundled them into suites. This consolidation has consequences:

- **Cross-selling pressure**: Once a city uses one product in a suite, the vendor pushes additional products with integration advantages
- **Switching cost compounding**: Leaving one product means disrupting integrations with the others
- **Pricing leverage**: As the vendor's footprint grows within a municipality, their negotiating position strengthens at each renewal
- **Innovation slowdown**: Acquired products often receive less investment post-acquisition, maintained at "good enough" rather than advancing with the market

### The AI Layer Complication

As AI capabilities are added to municipal software, the vendor dependency question deepens:

- **Model lock-in**: If a vendor's AI features are hardcoded to a single model provider (e.g., OpenAI), the municipality inherits that dependency without choosing it
- **Opaque reasoning**: Proprietary AI features may not allow the municipality to inspect how decisions or recommendations are generated
- **Training data questions**: What data was the system trained on? Does the vendor use municipal data to improve models for other clients?
- **Update cadence mismatch**: Underlying AI models improve rapidly (new modalities, better reasoning, lower costs), but vendor product updates follow their own slower roadmap

## Technical Decomposition Framework

### Purpose

Most municipal software can be decomposed into functional components, many of which are commodity capabilities available from multiple sources or as open-source. The decomposition makes the vendor's value proposition legible — revealing where the vendor provides genuine value and where they're applying markup to commodity infrastructure.

### Component Categories

**Commodity**: Well-understood capability available as open-source or from multiple commercial providers. Examples: web forms, email delivery, PDF generation, database storage, user authentication, basic reporting dashboards.

**Configured commodity**: Standard technology that requires meaningful domain-specific configuration for a government context. Examples: workflow engines configured with statutory deadlines, form builders configured with jurisdiction-specific fields, notification systems configured with compliance-required language.

**Proprietary**: Genuinely unique vendor capability that would be expensive or difficult to replicate. Examples: specialized compliance certifications (CJIS, FedRAMP), deep integrations with other government platforms, proprietary algorithms trained on large datasets. *Genuine proprietary value is rarer than vendors suggest.*

**Markup**: The vendor is reselling or wrapping a third-party service at a premium. Examples: AI chatbot wrapping an OpenAI API, SMS service wrapping a Twilio API, analytics wrapping a standard BI tool. The vendor's value-add is the government-context configuration and support, not the underlying technology.

### Decomposition Process

1. **List the deliverables** from the contract scope of work
2. **For each deliverable**, identify the core technical function (what does it actually do?)
3. **Categorize** each function: commodity, configured commodity, proprietary, or markup
4. **Identify open-source alternatives** for commodity and configured-commodity components
5. **Assess the vendor's value-add** — what, specifically, does the vendor provide beyond the commodity capability?
6. **Estimate the configuration gap** — how much work would it take to achieve the vendor's configuration using open tools?

### Common Decomposition Patterns

**Municipal chatbot** (e.g., Citibot, Polly):
| Component | Category | Notes |
|-----------|----------|-------|
| LLM / NLP engine | Markup | Wraps OpenAI, Anthropic, or similar API |
| RAG pipeline over city content | Configured commodity | Vector DB + embeddings + prompt engineering |
| Web chat widget | Commodity | Many open-source options |
| SMS integration | Markup | Wraps Twilio or similar |
| CRM integration | Configured commodity | API integration with Salesforce, etc. |
| Analytics dashboard | Commodity | Standard BI/reporting |
| Multi-language support | Markup | Wraps model's native multilingual capability |
| Government-context training | Configured commodity | Prompt engineering + content curation |

**FOIA management system** (e.g., NextRequest, GovQA):
| Component | Category | Notes |
|-----------|----------|-------|
| Request intake form | Commodity | Web form with file upload |
| Workflow/state machine | Configured commodity | Request lifecycle with statutory deadlines |
| Document management | Commodity | File storage with metadata |
| Redaction tools | Configured commodity | PDF redaction with exemption tagging |
| Public portal | Commodity | Status page with search |
| Email notifications | Commodity | Triggered emails |
| Reporting | Commodity | Standard dashboards |
| Compliance engine | Configured commodity | Jurisdiction-specific deadline calculations |

**Agenda management** (e.g., Legistar, Granicus):
| Component | Category | Notes |
|-----------|----------|-------|
| Agenda builder | Configured commodity | Document assembly with templates |
| Legislation tracking | Configured commodity | Version history and workflow |
| Public portal | Commodity | Searchable archive |
| Meeting minutes | Commodity | Document templates |
| Voting record | Commodity | Roll call tracking |
| Video integration | Markup | Wraps streaming/archival service |

## Lock-In Evaluation Framework

### The Five Dimensions of Vendor Lock-In

**1. Data lock-in**: Can the municipality extract its data in a usable format?
- Does the vendor provide a documented export API?
- What formats are available (open standards vs. proprietary)?
- Does the export include all data (records, metadata, audit trails, configurations)?
- What does the vendor's contract say about data ownership at termination?
- Is there a cost associated with data export?

**2. Platform lock-in**: How deeply is the vendor embedded in the municipality's operations?
- How many other systems integrate with this product?
- Would switching require reconfiguring adjacent systems?
- Are staff trained on vendor-specific interfaces that don't transfer?
- Does the vendor provide single sign-on or identity management that other systems depend on?

**3. Contract lock-in**: Do the contract terms favor the vendor at renewal?
- Auto-renewal provisions (what's the opt-out window?)
- Price escalation mechanisms (CPI-based? Uncapped?)
- Early termination penalties
- Multi-year commitments that outlast the underlying technology cycle

**4. Knowledge lock-in**: Does the vendor hold institutional knowledge the municipality can't access?
- Configuration settings that aren't documented or exportable
- Workflow customizations that live in the vendor's platform, not in municipal documentation
- Historical data about system performance and usage patterns
- Vendor staff who understand the municipality's setup better than municipal staff do

**5. Compliance lock-in**: Does the vendor's certification create regulatory dependency?
- CJIS, HIPAA, FedRAMP certifications that are expensive for alternatives to obtain
- Compliance documentation that references the specific vendor's architecture
- Audit trails that would need to be reconstructed during a migration

### Lock-In Classification

🔴 **Critical**: The municipality cannot leave this vendor without significant data loss, service disruption, or cost. Immediate concerns include: vendor claims ownership of usage data, no export API exists, proprietary data formats with no migration path, or termination would violate compliance requirements.

🟡 **Moderate**: Switching is feasible but costly. The municipality could leave, but it would require meaningful effort: re-implementation of workflows, staff retraining, data migration from limited export tools, or renegotiation of adjacent integrations.

🟢 **Low**: The municipality retains effective control. Data is exportable in open formats, APIs are documented, contract terms are reasonable, and switching to an alternative would be a planned project rather than a crisis.

## Build-vs-Buy Decision Framework

### When Building Makes Sense

- The capability is primarily commodity technology with government-context configuration
- The municipality has technically capable staff (or access to a technical partner)
- The vendor's price significantly exceeds the cost of the underlying components
- Data ownership and portability are priorities
- The municipality wants to adapt the system faster than the vendor's update cadence allows
- The use case is shared across many municipalities (community-maintained open-source is viable)

### When Buying Makes Sense

- The capability requires genuine proprietary technology or specialized compliance
- The municipality has no technical staff and no access to a technical partner
- The vendor's price is reasonable relative to the value and the build-maintain cost
- Time-to-deploy is critical and the vendor can deliver faster
- The vendor's support relationship provides real value for a small staff

### When the Third Option Makes Sense

Open-source infrastructure with a support partner — not vendor-dependent, not fully DIY:

- The municipality wants to own and control the system but needs implementation help
- The capability is well-suited to open-source (commodity + configuration)
- A support organization exists that can provide maintenance and updates
- The municipality values data ownership, model agnosticism, and long-term cost control
- The staff is willing to build capacity over time rather than outsource permanently

### Build Feasibility Assessment Criteria

| Factor | Question | Assessment |
|--------|----------|------------|
| Technical complexity | How many components are commodity vs. proprietary? | [Count and ratio] |
| Staff capacity | Does the municipality have staff who could maintain this? | [Yes/With training/No] |
| Maintenance burden | What's the ongoing effort to keep it running? | [Hours/month estimate] |
| Risk tolerance | What happens if the build fails or stalls? | [Fallback plan] |
| Time horizon | How soon is the capability needed? | [Weeks/Months/Flexible] |
| Community support | Is there an active open-source community around the components? | [Strong/Moderate/Minimal] |

### Cost Comparison Methodology

Always compare total cost of ownership over a **3-year and 5-year horizon**:

**Vendor costs include:**
- Implementation/setup fees
- Annual subscription
- Price escalation over term
- Per-seat or per-unit overages
- Add-on modules (redaction, SSO, storage, analytics)
- Integration maintenance
- Training (initial and ongoing for staff turnover)

**Build costs include:**
- Development time (internal staff or contractor)
- Infrastructure (hosting, storage, domain, SSL)
- Third-party services (API costs for AI models, SMS, email delivery)
- Ongoing maintenance and updates
- Staff training and documentation
- Opportunity cost of staff time diverted from other work

**What cost comparisons don't capture** (always disclose):
- Value of vendor support relationship (someone to call)
- Risk premium for self-managed systems
- Staff learning curve and productivity dip during transition
- Potential for scope creep in custom builds
- Political cost of a failed build attempt

## AI-Specific Assessment Criteria

When the vendor's product includes AI or machine learning capabilities, apply these additional evaluation criteria:

### Model Dependency
- Is the system locked to a single AI model provider?
- Can the municipality (or the vendor) switch models if a better option emerges?
- What happens to the system if the underlying model provider changes pricing, terms, or capabilities?

### Transparency
- Can the municipality see what data the AI was trained on?
- Are the AI's outputs explainable — can a staff member understand why the system produced a particular response?
- Does the vendor provide confidence scores or citations for AI-generated content?

### Data Usage
- Does the vendor use municipal data to train or improve models for other clients?
- Are resident interactions with AI systems logged and retained? By whom? For how long?
- Is resident data shared with the underlying model provider?

### Update Cadence
- How quickly does the vendor incorporate improvements in underlying AI models?
- Are new modalities (voice, vision, file upload) available as the underlying models add them?
- Who controls the update timeline — the municipality, the vendor, or the model provider?

### Cost Structure
- What is the vendor's AI cost relative to the underlying API cost? (This reveals the markup)
- Are there per-query or per-token costs that could scale unexpectedly?
- Does the vendor pass through model provider price decreases or only price increases?

## Contract Term Red Flags

### Terms That Should Concern Any Municipality

| Term | Why It Matters | What to Negotiate |
|------|---------------|-------------------|
| "Usage data is the confidential information of [Vendor]" | The vendor claims ownership of data about how your staff and residents use the system | Usage data should be the municipality's data |
| "Data will not be provided under any circumstances" after deletion | No data portability at contract end | Require data export in open formats upon termination |
| Auto-renewal with <90 day opt-out | Easy to miss the window and be locked in for another term | Require 180-day notice and explicit renewal approval |
| Uncapped annual escalation | Costs can increase unpredictably | Cap escalation at CPI or a fixed percentage |
| No API or "API available at additional cost" | Your data is trapped unless you pay more | Require API access as part of the base subscription |
| Vendor may modify terms unilaterally | The contract you signed isn't the contract you'll have next year | Require mutual agreement for term changes |
| Indemnification is one-sided (municipality indemnifies vendor) | The municipality bears legal risk for the vendor's product | Require mutual indemnification |

### Terms That Are Reasonable

- Annual price adjustments tied to CPI (typically 2-4%)
- Vendor retains IP in the software platform itself (not in your data or configurations)
- Uptime SLAs with defined remedies for failures
- Defined support response times
- Reasonable limitation of liability (typically capped at contract value)

## Using Connected Tools

Use `municipal-code` to look up procurement-related code provisions. See the `municipal-code-analysis` skill for the full MunicipalMCP tool reference.

**Common search patterns for vendor and procurement analysis** (use `search_municipal_codes` with these queries):
- Procurement thresholds: `"purchasing"`, `"procurement"`, `"competitive bidding"`, `"bid threshold"`
- Contract approval: `"contract approval"`, `"council approval"`, `"city manager authority"`
- Technology and IT: `"information technology"`, `"technology services"`, `"software"`
- Data and records: `"data ownership"`, `"public records"`, `"electronic records"`
- Professional services: `"professional services"`, `"consulting"`, `"sole source"`

**Workflow tip**: Procurement thresholds and approval requirements are often in the Administration or Finance title. Use `titles_only=true` to locate them.

Use web search to research vendors — product capabilities, pricing tiers, client base, and recent news about acquisitions or product changes.

When connected tools are unavailable or unauthenticated, work from uploaded contract documents and web search. Do not imply prior contracts, procurement documents, or historical approvals were reviewed unless they were actually retrieved.

**Optional and planned connectors** (plugin works without these):
- `document-management` (optional/configured when authenticated) — prior contracts and procurement documents
- `agenda-management` (planned; not yet implemented in this plugin) — historical contract approvals

## Municipal Configuration

Check `municipal.local.md` for:
- Contract authority thresholds
- Procurement ordinance requirements
- Current technology contracts
- IT staffing and capacity
- Budget context for technology spending
- Policy priorities related to technology, transparency, or digital services

## Related Skills

- `public-finance` — contract cost analysis, budget impact assessment, fiscal thresholds for classification
- `policy-evaluation` — evaluation criteria frameworks, decision matrices, stakeholder analysis for procurement decisions
- `municipal-code-analysis` — procurement ordinance interpretation, contract approval requirements, purchasing authority
- `open-meetings-foia` — public records implications of vendor-held data, FOIA obligations for AI-generated communications with residents

## Key Questions for Vendor Assessment

1. What are you actually paying for? (Decompose the deliverable)
2. How much of this is commodity technology?
3. What does the vendor add beyond the commodity capability?
4. Can you get your data out? In what format?
5. What happens to your data if the contract ends?
6. Is the system locked to a single AI model?
7. How quickly does the vendor ship improvements available in the underlying technology?
8. What would it cost to build this with open-source tools?
9. Does your staff have (or could they develop) the capacity to maintain an alternative?
10. What are the switching costs if you decide to leave?
