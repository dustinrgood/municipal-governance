# Municipal Governance Plugin

AI-powered productivity tools for local government elected officials and staff.

## Overview

This plugin automates common workflows for municipal elected officials and staff:

- **Ordinance Analysis**: Review proposed ordinances against existing municipal code
- **Meeting Preparation**: Generate briefings from agenda packets
- **Policy Research**: Multi-source research with intergovernmental context
- **Budget Review**: Analyze fiscal impacts and budget items
- **Constituent Communications**: Draft responses following municipal communication standards
- **Agenda Synthesis**: Quick-turnaround agenda packet summaries
- **Intergovernmental Scanning**: Monitor state/federal policy impacts on local government
- **Vendor Evaluation**: Analyze vendor contracts, decompose deliverables, assess lock-in, and produce build feasibility specs
- **Meeting Close-Out**: Capture votes, follow-ups, and surprises after meetings for persistent institutional memory
- **PolicyAide Sync**: Send local profile/context summaries to PolicyAide for deeper multi-agent research
- **Watcher Workflows**: Monitor agendas, statehouse activity, vendor renewals, grants, and council follow-ups

## Quick Start

1. **Install** the plugin
2. **Customize** — Click "Customize plugin settings" to configure your municipality
3. **Create workspace** — Run the **Setup Project** agent to scaffold a Cowork Project folder
4. **Try it** — Upload an agenda packet and run `/municipal-governance:agenda-synthesis`

## Installation

### Cowork (Claude Desktop)

1. Download or clone this repository
2. In Cowork, go to **Settings > Plugins > Install from folder**
3. Select the `CivicWorkPlugin` directory
4. The plugin's commands will appear as `/municipal-governance:*`

### Claude Code (CLI)

1. Clone this repository
2. Open Claude Code in the repository directory — plugin files are detected automatically

### Setup (Cowork)

1. **Configure your municipality** — Click **"Customize plugin settings"** on the Cowork home screen. Cowork auto-discovers most municipality info via web search (population, government type, council structure, code provider, meeting schedule) and asks you about what it can't find. This writes `municipal.local.md`.

2. **Create a project workspace** — Run the **Setup Project** agent from the Agents tab. It creates a folder on your computer with the right structure for governance work:

   ```
   ~/Municipal-Governance/{your-city}/
   ├── CLAUDE.md              ← Project instructions (auto-generated)
   ├── documents/
   │   ├── agendas/           ← Drop agenda packets here
   │   ├── budgets/           ← Budget documents, AFRs
   │   ├── contracts/         ← Vendor contracts and renewal notices
   │   ├── ordinances/        ← Proposed ordinances under review
   │   └── plans/             ← Comprehensive plan, strategic plan, CIP
   ├── meeting-notes/         ← Post-meeting summaries and action items
   ├── research/              ← Saved policy research
   └── watchers/              ← Local watcher memory and status logs
   ```

   Then point Cowork to it: **"Work in a project" → "+" → "Use an existing folder"** → select the created folder.

3. **(Optional) Configure connectors** — See [Connectors](#connectors) below to enable live municipal code lookup.

### Setup (Claude Code CLI)

1. Run the **Setup Municipality** agent — it walks you through the full configuration interactively and offers to create a project workspace at the end.
2. Or edit `municipal.local.md` manually.

### Why a Project Workspace?

The plugin provides analytical tools. The project workspace provides **persistent memory**. With a Cowork Project, Claude remembers prior meeting decisions, research, and your preferences across sessions — building institutional knowledge over time. Without it, every conversation starts fresh.

## Commands

| Command | Description |
|---------|-------------|
| `/municipal-governance:analyze-ordinance` | Analyze proposed ordinances against municipal code |
| `/municipal-governance:meeting-prep` | Prepare comprehensive briefings for council/committee meetings |
| `/municipal-governance:policy-research` | Multi-source policy research workflow |
| `/municipal-governance:agenda-synthesis` | Quick synthesis of agenda packets into briefings |
| `/municipal-governance:constituent-response` | Draft constituent communications |
| `/municipal-governance:budget-review` | Analyze budget items and fiscal impact |
| `/municipal-governance:intergovernmental-scan` | Scan state/federal policy impacts |
| `/municipal-governance:vendor-evaluate` | Evaluate vendor contracts, assess lock-in, and spec open-source alternatives |
| `/municipal-governance:meeting-close-out` | Record meeting outcomes, follow-ups, and briefing-book updates |
| `/municipal-governance:sync-to-policyaide` | Sync municipality, official profile, and standing document summaries to PolicyAide |
| `/municipal-governance:skill-qa` | Review plugin skills for provenance, trust gates, stale references, and connector assumptions |
| `/municipal-governance:agenda-watcher` | Monitor upcoming agendas, packets, addenda, and agenda changes |
| `/municipal-governance:statehouse-monitor` | Track state/federal bills, mandates, preemption risks, and agency updates |
| `/municipal-governance:vendor-renewal-watcher` | Watch contract renewals, auto-renewal windows, escalation, and data-export deadlines |
| `/municipal-governance:grant-radar` | Monitor grants, NOFOs, deadlines, eligibility, match, and strategic fit |
| `/municipal-governance:council-follow-up-tracker` | Track staff directives, deferred items, promised reports, and open meeting follow-ups |

## Configuration

The plugin uses a three-layer architecture:

| Layer | File | What it contains |
|-------|------|-----------------|
| **Municipality config** | `municipal.local.md` | Your council structure, contacts, fiscal context, priorities |
| **State reference** | `state-references/{STATE}.md` | Statutory requirements, deadlines, thresholds, penalties |
| **Skills** | `skills/*/SKILL.md` | Analytical frameworks (state-agnostic) |

**`municipal.local.md`** is created when you customize the plugin. It includes: municipality name, state, government type, council structure, meeting schedule, code provider, budget context, fiscal thresholds, policy priorities, contacts, and committee structure.

**State references** provide jurisdiction-specific statutory content — statute citations, compliance deadlines, dollar thresholds, penalty structures, and institutional resources. Currently available: **Illinois** (`state-references/IL.md`). Additional states can be contributed by following the IL.md structure as a template.

The plugin works for **any US municipality**. State references add precision for compliance-critical areas; without one for your state, skills provide general framework guidance with a recommendation to verify state-specific requirements with legal counsel.

## Skills

The plugin includes 11 domain expertise modules. You don't invoke these directly — Claude activates them automatically based on what you're working on.

| Skill | What it covers | Example prompt |
|-------|---------------|----------------|
| `municipal-code-analysis` | Code interpretation, cross-referencing, MunicipalMCP tool reference | "Does this amendment conflict with Chapter 19?" |
| `parliamentary-procedure` | Robert's Rules, scripted chair language, quasi-judicial hearings | "What do I say to open a public hearing?" |
| `land-use-zoning` | Zoning, variances, TIF districts, form-based codes, inclusionary zoning | "Walk me through the variance criteria for this parcel" |
| `public-finance` | Budgets, fund accounting, debt, pension/OPEB, CIP planning | "What's the pension impact of this staffing proposal?" |
| `intergovernmental-relations` | Home rule, preemption decision tree, IGA evaluation, grant scoring | "Should we pursue this grant opportunity?" |
| `policy-evaluation` | Bardach framework, logic models, decision matrices, stakeholder analysis | "Evaluate these three approaches to short-term rentals" |
| `open-meetings-foia` | OMA, FOIA exemption decision tree, closed session framework, response templates | "Can we discuss this in executive session?" |
| `council-communication` | Staff reports, ordinances vs resolutions, legal drafting, constituent triage | "Should this be an ordinance or a resolution?" |
| `ethics-conflicts` | Conflicts of interest, recusal procedures, gift rules, financial disclosure | "Do I need to recuse myself from this vote?" |
| `vendor-assessment` | Vendor lock-in, build-vs-buy, technical decomposition, procurement analysis | "Should we renew this contract or build our own?" |
| `vendor-alternatives` | Municipal software alternatives, replacement tiers, open-source landscape | "What open-source options exist for agenda management?" |

## Connectors

The plugin can optionally connect to external data sources via MCP servers. Currently configured:

| Connector | Description | Provider |
|-----------|-------------|----------|
| `municipal-code` | Search and retrieve municipal code sections (7 tools) | [MunicipalMCP](https://github.com/Skatterbrainz/MunicipalMCP) (Municode API) |
| `document-management` | Retrieve agenda packets, staff reports, FOIA records, and standing documents when available | Box MCP HTTP endpoint (`https://mcp.box.com`) |

`municipal-code` requires a local MunicipalMCP installation and machine-specific paths in `.mcp.json`. `document-management` is configured as an optional Box MCP endpoint and depends on account access/authentication. Without connectors, commands work with uploaded documents and clearly marked web or model-derived assumptions.

## How It Works

1. You run a command (e.g., `/municipal-governance:meeting-prep`) and upload a document
2. Claude asks a few quick scoping questions — what items matter to you, how deep should the analysis go
3. Claude loads your municipality's configuration from `municipal.local.md` and the applicable state reference from `state-references/`
4. Relevant skills activate automatically based on the subject matter
5. You get structured output with attention-priority flags (red/yellow/green), confidence and provenance indicators on key claims, and recommended next steps — matched to the depth you asked for
6. For high-stakes items, the output includes an **Analysis Boundaries** section — a transparent disclosure of what the analysis can and cannot responsibly conclude as a single AI instance, with specific recommendations for verification (attorney review, staff confirmation, or escalation to PolicyAide's multi-agent deliberation framework)

### Why Analysis Boundaries Matter

Most AI tools pretend they can handle everything. This plugin tells you when it's not enough. A single AI instance is great for research, organization, and synthesis — but policy decisions with real fiscal, legal, or community impact deserve adversarial testing, independent verification, and human judgment. The plugin knows its own limits and says so.

## License

Apache 2.0 License — Built by [CivicWork](https://civicwork.ai)
