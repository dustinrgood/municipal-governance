# Setup Project Workspace

Creates a Cowork Project folder for your municipality's governance work, enabling persistent memory across sessions.

## Role

This agent reads your municipality's configuration from `municipal.local.md` and scaffolds a local project workspace with the right structure and instructions. After creation, you point Cowork's "Work in a project" feature to this folder for a persistent governance workspace with cross-session memory.

## When to Use

Run this agent:
- After installing the plugin and configuring your municipality (via "Customize plugin settings" or the setup-municipality agent)
- To set up a persistent workspace with cross-session memory in Cowork

## Prerequisites

`municipal.local.md` must be configured with at least a municipality name and state. If it's not configured yet, run "Customize plugin settings" first or use the setup-municipality agent.

## Instructions

### Step 1: Read Configuration

Read `municipal.local.md` in this plugin's directory. Extract:
- Municipality name
- State
- Government type
- Council/board structure
- Key contacts (if available)
- Current priorities (if available)
- Fiscal year
- Meeting schedule

If `municipal.local.md` is missing or unconfigured (still has bracket placeholders for name and state), tell the user to configure it first and stop.

### Step 2: Choose Location

Ask the user:

> "I'll create a governance workspace for **{municipality name}**. Where would you like it?
>
> **Default**: `~/Municipal-Governance/{city-name}/`
>
> Press Enter for the default, or type a different path."

Use the default if the user accepts. Sanitize the city name for the folder (lowercase, hyphens for spaces, no special characters).

### Step 3: Create Folder Structure

Create:

```
{workspace}/
├── CLAUDE.md
├── documents/
│   ├── agendas/
│   ├── budgets/
│   ├── contracts/
│   ├── ordinances/
│   └── plans/
├── meeting-notes/
├── research/
└── watchers/
    ├── agenda-watcher.md
    ├── statehouse-monitor.md
    ├── vendor-renewal-watcher.md
    ├── grant-radar.md
    └── council-follow-up-tracker.md
```

Create the watcher memory files from the canonical starter templates in `templates/watchers/`:

- `templates/watchers/agenda-watcher.md` → `{workspace}/watchers/agenda-watcher.md`
- `templates/watchers/statehouse-monitor.md` → `{workspace}/watchers/statehouse-monitor.md`
- `templates/watchers/vendor-renewal-watcher.md` → `{workspace}/watchers/vendor-renewal-watcher.md`
- `templates/watchers/grant-radar.md` → `{workspace}/watchers/grant-radar.md`
- `templates/watchers/council-follow-up-tracker.md` → `{workspace}/watchers/council-follow-up-tracker.md`

If a template file is unavailable, create a starter Markdown file with these sections: Purpose, Last Checked, Sources Watched, Current Watchlist/Open Items, Recent Changes, Next Check, and Notes / Verification Needed. Include a public-record caution in the notes section and keep external actions draft-only unless the user confirms.

### Step 4: Generate Project CLAUDE.md

Write a `CLAUDE.md` in the workspace root, populated from `municipal.local.md` data. This file is loaded into every conversation in the project — keep it concise.

Use this template, filling in values from municipal.local.md (use the placeholder note for any fields that are still bracket placeholders):

```markdown
# {Municipality Name} — Municipal Governance Workspace

Powered by the **municipal-governance** plugin from [CivicWork](https://civicwork.ai).
Memory is enabled — Claude builds institutional knowledge across sessions in this project.

## Municipality

- **Name**: {name}
- **State**: {state}
- **Government**: {government type}
- **Home Rule**: {yes/no/unknown}
- **Population**: {if available}

## Standing Instructions

When using `/municipal-governance:*` commands in this project:
1. Load the state reference from the plugin (`state-references/{STATE}.md`) for jurisdiction-specific statutory requirements
2. Check `meeting-notes/` for context from prior meetings before preparing new meeting briefs
3. Reference `documents/plans/` for comprehensive plan and strategic plan alignment checks
4. Save policy research outputs to `research/` for future reference
5. After meetings, use `/municipal-governance:meeting-close-out` and save results to `meeting-notes/`
6. Store watcher baselines and run logs in the canonical `watchers/*.md` memory files only after reviewing proposed updates

## Workspace

| Folder | What goes here |
|--------|---------------|
| `documents/agendas/` | Agenda packets for meeting-prep and agenda-synthesis |
| `documents/budgets/` | Budget documents, AFRs, financial reports |
| `documents/contracts/` | Vendor contracts, amendments, renewal notices, and RFP materials |
| `documents/ordinances/` | Proposed ordinances under review |
| `documents/plans/` | Comprehensive plan, strategic plan, CIP |
| `meeting-notes/` | Post-meeting summaries, decisions, action items |
| `research/` | Saved policy research for future reference |
| `watchers/` | Canonical watcher memory files for agendas, statehouse activity, vendor renewals, grants, and council follow-ups |

## Key Contacts

{If contacts found in municipal.local.md, list them. Otherwise:}
<!-- Fill in key staff contacts: city manager, clerk, attorney, finance director, FOIA officer -->

## Current Priorities

{If priorities found in municipal.local.md, list them. Otherwise:}
<!-- Fill in 3-5 current council/administration priorities -->

## Meeting Schedule

{If schedule found in municipal.local.md, include it. Otherwise:}
<!-- Fill in regular meeting day, time, and location -->
```

### Step 5: Tell the User What to Do Next

After creating the workspace:

> **Your governance workspace is ready at `{path}/`**
>
> **To activate it as a Cowork Project:**
> 1. Click **"Work in a project"** at the top of the Cowork chat
> 2. Click the **+** button
> 3. Select **"Use an existing folder"**
> 4. Navigate to **`{path}/`** and select it
>
> From then on, start your governance work by selecting this project from the "Work in a project" dropdown. Claude will have persistent memory across sessions — it'll remember prior meeting decisions, research, and your preferences.
>
> **Quick start:** Drop an agenda packet PDF into `documents/agendas/` and run `/municipal-governance:agenda-synthesis`.

### Important Guidelines

- Keep this fast — under 1 minute for the entire process
- Use defaults unless the user objects. Don't ask unnecessary questions.
- If municipal.local.md has incomplete data, still create the workspace — note which sections need filling in
- The project CLAUDE.md must stay concise — it's loaded into every conversation
