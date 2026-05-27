# AGENTS.md

This file provides guidance to Codex (Codex.ai/code) when working with code in this repository.

## Project Overview

This is a **Codex AI plugin** (`municipal-governance`, v0.6.0) for local government officials and staff, designed to work for **any US municipality**. Built by Dustin Good, sitting Elgin Illinois Councilmember and creator of CivicAide, PolicyAide, and CivicWork.Ai.

It provides automated workflows for ordinance analysis, meeting preparation, policy research, budget review, constituent communications, agenda synthesis, intergovernmental scanning, vendor evaluation, meeting close-out, PolicyAide sync, skill quality review, and watcher workflows for agendas, statehouse activity, vendor renewals, grants, and council follow-ups.

**Key characteristic**: This is a prompt/workflow-based plugin using Markdown and JSON configuration — not compiled code. There are no build steps, tests, or linting tools.

## Architecture

```
.
├── agents/             # 3 utility agents (setup-municipality, setup-project, setup-official)
├── skills/             # 27 skills: 16 workflow commands + 11 domain expertise modules
├── state-references/   # State-specific statutory requirements (IL.md, etc.)
├── hooks/              # Event hooks (SessionStart config check)
├── scripts/            # Hook scripts (check-config.sh)
├── .claude-plugin/     # Plugin metadata (plugin.json v0.6.0)
├── .claude/            # Local Claude settings (enabled MCP servers)
├── .mcp.json           # MCP server connections (municipal-code, document-management)
├── municipal.local.md  # Municipality-specific configuration (template — customize per deployment)
├── official.local.md   # Elected official's personal profile (template — created by setup-official agent)
├── README.md           # User documentation with installation + quick start
└── CivicWorkPluginReference.md  # Developer reference
```

### Design

**Agents** (`/agents/`): Utility agents that run as Codex subprocesses in Cowork with file editing access:
- `setup-municipality` — Interactive configuration wizard that walks users through customizing `municipal.local.md` (primarily for Codex CLI users; Cowork users can use the native "Customize plugin settings" instead)
- `setup-official` — Captures the elected official's personal profile (platform, priorities, campaign positions) via auto-discovery + interview, plus auto-discovers standing documents (strategic plan, comprehensive plan, budget). Writes `official.local.md` and `standing-documents.md`.
- `setup-project` — Scaffolds a Cowork Project workspace folder with the right directory structure and a project-level AGENTS.md, enabling persistent memory across sessions

**Skills** (`/skills/*/SKILL.md`) form two tiers of domain expertise, all in the same directory format:

1. **Workflow Skills** (16): User-facing workflows invoked via `/municipal-governance:*`. Each workflow skill:
   - **Scopes the work first** — brief questions to focus depth and identify what the user actually needs (see "Scope" step)
   - Loads `municipal.local.md` configuration
   - References specific domain skills via `## Skills Referenced` section
   - Produces structured Markdown output with three-tier attention indicators (🔴/🟡/🟢), confidence tags, and provenance tags on key claims
   - Supports **depth modes** (e.g., quick scan vs. comprehensive) — set during scoping
   - Lists `## Related Skills` for discoverability
   - Includes "omit if N/A" guidance on longer output templates

2. **Domain Skills** (11): Knowledge modules that workflow skills draw on, containing:
   - YAML frontmatter with `description` (triggers automatic activation)
   - Frameworks, methodologies, analysis techniques
   - `## Related Skills` cross-references to other skills
   - `## Using Connected Tools` with active tools and clearly marked planned connectors
   - `## Municipal Configuration` listing relevant `municipal.local.md` fields
   - Three domain skills include structured output templates: `policy-evaluation`, `public-finance`, `municipal-code-analysis`

### Workflow Skills (16)

| Skill | Purpose |
|-------|---------|
| `analyze-ordinance` | Review ordinances against municipal code |
| `meeting-prep` | Comprehensive meeting briefings |
| `policy-research` | Multi-source policy research |
| `agenda-synthesis` | Quick agenda packet summaries |
| `constituent-response` | Draft constituent communications |
| `budget-review` | Fiscal impact and budget analysis |
| `intergovernmental-scan` | State/federal policy monitoring |
| `vendor-evaluate` | Analyze vendor contracts and assess alternatives |
| `sync-to-policyaide` | Sync profile, municipality context, and standing docs to PolicyAide |
| `meeting-close-out` | Capture meeting outcomes, votes, follow-ups, and briefing-book updates |
| `skill-qa` | Review skills for provenance, analysis boundaries, freshness, and connector safety |
| `agenda-watcher` | Monitor agenda postings, packets, addenda, and agenda changes |
| `statehouse-monitor` | Track legislative, agency, mandate, preemption, and funding developments |
| `vendor-renewal-watcher` | Watch vendor renewal windows, auto-renewals, escalation, and data-export deadlines |
| `grant-radar` | Monitor grant opportunities, NOFOs, deadlines, eligibility, and strategic fit |
| `council-follow-up-tracker` | Track staff directives, deferred items, promised reports, and open follow-ups |

### Domain Skills (11)

| Skill | Purpose |
|-------|---------|
| `municipal-code-analysis` | Code interpretation, cross-referencing, hierarchy of authority |
| `parliamentary-procedure` | Robert's Rules, motions, voting procedures |
| `land-use-zoning` | Zoning, variances, form-based codes, inclusionary zoning, TIF |
| `public-finance` | Fund accounting, budgets, debt, pension/OPEB, CIP, fiscal impact |
| `intergovernmental-relations` | Home rule, preemption, grants, regional cooperation |
| `policy-evaluation` | Bardach framework, logic models, stakeholder analysis, decision matrices |
| `open-meetings-foia` | OMA compliance, FOIA, virtual meetings, records retention, social media |
| `council-communication` | Staff reports, ordinances, resolutions, minutes, amendment conventions |
| `ethics-conflicts` | Conflict of interest, recusal, gift restrictions, financial disclosure |
| `vendor-assessment` | Vendor lock-in, build-vs-buy, technical decomposition, procurement |
| `vendor-alternatives` | Municipal software alternatives knowledge base, replacement tiers, open-source landscape |

### Data Flow

```
User command/watch run → Scope the work (brief user questions) → Load municipal.local.md → Load state reference (state-references/{state}.md) → Retrieve documents (upload or MCP) → Reference skills → Structured Markdown output with confidence/provenance indicators
```

### MCP Server Connections

Defined in `.mcp.json`. **Configured:**
- `municipal-code` — Municode lookup via [MunicipalMCP](https://github.com/Skatterbrainz/MunicipalMCP) (Python, installed at `~/CivicWork/MunicipalMCP` with Python 3.12 venv). Path is machine-specific — must be updated per-installation.
- `document-management` — Box HTTP MCP endpoint (`https://mcp.box.com`) for agenda packets, staff reports, FOIA records, and standing documents when the user's Box access/authentication is available. Treat as optional/experimental and provide uploaded-document fallback.

**Planned** (referenced in skills as "Planned connectors" — plugin works without these):
- `agenda-management` — legislation tracking, meeting schedules
- `communication` — team messaging
- `project-tracking` — action item tracking

### Hooks

Defined in `hooks/hooks.json`. The plugin uses event hooks for automated checks:

- **SessionStart** — `scripts/check-config.sh` runs at the start of every session. Checks three things:
  1. Does `municipal.local.md` exist? If not → prompts user to configure
  2. Is the state field populated? If not → warns
  3. Does a state reference exist for the configured state? If not → warns that compliance guidance will be general only

The hook is silent on the happy path (configured municipality with state reference available).

## Working with This Codebase

### Configuration Is Central

All workflows depend on `municipal.local.md` being customized with municipality-specific information. The template includes sections for: municipality basics, council structure, code references, agenda management, TIF districts, budget context, fiscal impact thresholds, policy priorities, ethics/disclosure rules, contacts, committees, procedural notes, and regional context.

### Adding New Workflow Skills

Follow the existing pattern in `skills/your-skill-name/SKILL.md`:
1. Define trigger and description
2. Specify required inputs
3. Start with a **Scoping step** — brief questions to focus depth and identify what the user needs
4. Load Municipal Context step (load `municipal.local.md`)
5. Document step-by-step workflow
6. Include structured output template (with "omit if N/A" guidance for optional sections)
7. Include **Analysis Boundaries** section in output template if the skill produces analysis someone might act on (see "Single-Instance Disclosure" above)
8. Include confidence/provenance expectations for decision-relevant claims
9. Add `## Skills Referenced` listing primary and secondary skills
10. Add `## Related Skills` pointing to complementary skills
11. Add `## Notes` with edge cases and graceful degradation
12. Update README.md command table

### Adding New Skills

Create a new directory under `/skills/` with a `SKILL.md` file containing:
1. YAML frontmatter with `description` (tells Codex when to activate)
2. Overview and purpose
3. Conceptual framework / methodology
4. Analysis techniques with step-by-step procedures
5. Source, confidence, and provenance rules for decision-relevant claims
6. Output template (for skills producing formal analyses)
7. Quality standards and common pitfalls
8. `## Related Skills` cross-referencing 2-4 related skills
9. `## Using Connected Tools` (active tools + "Planned connectors" section)
10. `## Municipal Configuration` listing relevant `municipal.local.md` fields
11. State-reference freshness guidance when the skill touches law or compliance
12. Caveats and limitations
13. Update README.md skills table

### Output Conventions

All command outputs follow consistent patterns:
- Executive summary at top
- Three-tier attention indicators: 🔴 (needs close review) / 🟡 (standard review) / 🟢 (consent/routine)
- Tables for comparisons
- Legal disclaimers where appropriate
- "Next Steps" or "Recommended Actions" section
- Omit sections where data is unavailable rather than generating boilerplate

### Confidence and Provenance Indicators

Key substantive claims in command output should include confidence and provenance so the user knows both how sure the analysis is and where the claim came from.

- **High** — directly sourced from a provided document, municipal code lookup, or official record. Example: *"Fiscal impact: $2.3M one-time (High — staff report p.3)"*
- **Medium** — inferred from partial information, based on typical patterns, or sourced from web search. Example: *"May conflict with Title 19.07 (Medium — based on title text, no ordinance draft provided)"*
- **Low** — based on general knowledge, analogies to other municipalities, or incomplete data. Example: *"Implementation likely requires 2-3 FTEs (Low — estimate based on peer city experience, no local staffing data)"*

Provenance answers "where did this come from?" Use concise source tags such as:

- `[staff report p.4]`
- `[municipal-code]`
- `[state-reference/IL]`
- `[official site]`
- `[web - verify]`
- `[model inference]`

Confidence/provenance tags appear inline after the claim, in parentheses. Keep them brief. Don't tag every sentence — tag key facts that inform decisions: fiscal figures, legal conclusions, code conflict assessments, procedural requirements, and implementation estimates. Routine descriptive content (item titles, meeting dates, procedural steps) doesn't need tagging.

Example: *"Fiscal impact: $2.3M one-time (High; provenance: staff report p.3)."*

### Single-Instance Disclosure (Verification Line)

This plugin runs as a single AI instance — one model, one pass, one perspective. That's appropriate for many tasks (information gathering, document synthesis, procedural guidance) but insufficient for high-stakes policy decisions.

Commands must include a **verification disclosure** when their output approaches the boundary of what a single instance can responsibly provide. The disclosure:

1. **Acknowledges the limitation** — this analysis was not adversarially tested, independently verified, or produced through multi-perspective deliberation
2. **Identifies what specifically needs deeper treatment** — not a generic warning, but a list of the specific claims, conclusions, or recommendations that would benefit from further verification
3. **Points to the next level** — PolicyAide's multi-agent deliberation framework, attorney review, staff analysis, or other appropriate escalation

**When to include the disclosure:**
- Policy research in **Decision-focused** or **Comprehensive** mode
- Ordinance analysis classified as 🟡 SIGNIFICANT or 🔴 CRITICAL
- Meeting prep items flagged 🔴 that involve code amendments, significant fiscal impact, or legal risk
- Any output that includes a recommendation the user might act on directly

**When it's not needed:**
- Quick scans and exploratory research (informational, not decision-driving)
- Agenda synthesis (summary, not analysis)
- Constituent responses (communication, not policy)
- Factual lookups (what does the code say about X)

The disclosure is not a liability disclaimer — it's a design principle. The plugin knows its own limits and says so. This is what responsible AI deployment looks like in government.

### Cross-References

- Every skill has a `## Related Skills` section referencing 2-4 related skills
- Every command has `## Skills Referenced` (primary + secondary skills) and `## Related Skills`
- Maintain these when adding or modifying skills/commands
- See `CivicWorkPluginReference.md` for the full command → skill mapping table

### State-Specific Content — Two-Layer Architecture

Skills provide **state-agnostic frameworks** (analytical workflows, professional standards, output templates). State-specific statutory requirements live in **`state-references/`**.

```
Layer 1: Skills (frameworks)     — How to analyze. Same for all states.
Layer 2: State references        — What the law says. Unique per state.
Layer 3: municipal.local.md      — Municipality-specific config. Unique per deployment.
```

**State reference documents** (`state-references/{STATE}.md`) contain:
- Freshness metadata (`last_verified`, `freshness_window_days`, `verified_against`)
- Statutory citations with section numbers
- Deadlines, thresholds, and penalties
- Procedural requirements
- Common compliance failures
- Institutional resources (state league, comptroller, etc.)
- Preemption landscape

**When providing compliance guidance**, domain skills must:
1. Load the state reference for the user's state (from `municipal.local.md`)
2. Apply all statutory requirements, deadlines, and thresholds from the state reference
3. Check freshness metadata when present
4. If no state reference exists, note the limitation and recommend verification with legal counsel
5. If the state reference is stale, use it as background only and recommend verification against current statutes or qualified legal counsel before relying on it

**Currently available**: `state-references/IL.md` (Illinois). Additional states can be contributed by following the IL.md structure as a template.

**What goes where**:
- **Statute, deadline, threshold, penalty** → state reference
- **Framework, methodology, output template** → skill file
- **Council structure, contacts, fiscal context** → `municipal.local.md`

## Known Pitfalls

### plugin.json `author` field must be an object

Claude Desktop / Cowork plugin validation expects the `author` field to be an object with a `name` property. A plain string can fail validation and break plugin management.

```json
"author": "CivicWork"              // WRONG — breaks the Plugins page
"author": { "name": "CivicWork" }  // CORRECT
```

### MCP placeholder entries propagate and persist

When a plugin with `.mcp.json` entries is uploaded to Claude Desktop / Cowork, those entries can be written to multiple places:

1. The global Claude Desktop config (`~/Library/Application Support/Claude/claude_desktop_config.json`) — prefixed as `municipal-governance:<name>`
2. The marketplace copy inside `local-agent-mode-sessions/.../cowork_plugins/marketplaces/local-desktop-app-uploads/municipal-governance/.mcp.json`
3. The cache copy inside `local-agent-mode-sessions/.../cowork_plugins/cache/local-desktop-app-uploads/municipal-governance/<version>/.mcp.json`

If the project `.mcp.json` is later cleaned, the uploaded copies and global config are **not** updated automatically. Stale entries can cause persistent error banners in the host app. To fully remove them, all three locations must be cleaned manually.

### .mcp.json contains machine-specific paths and optional HTTP connectors

The current `.mcp.json` has `municipal-code` configured with absolute paths to the developer's local MunicipalMCP installation. This must be updated per-installation. It also has `document-management` configured as a Box HTTP MCP endpoint, which depends on account access/authentication and should be treated as optional. Other connector categories are referenced in skills as planned integrations but are not yet wired — the plugin works without them.
