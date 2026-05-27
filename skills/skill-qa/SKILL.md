---
description: >
  This skill should be used when reviewing the quality, safety, and maintainability
  of CivicWork Plugin skills or workflow commands. Triggers include skill QA,
  prompt QA, validate skills, review a skill, check plugin trust gates, or assess
  whether a skill has proper provenance, scoping, analysis boundaries, connector
  fallbacks, and state-reference freshness handling.
---

# Skill QA

Review one skill or the whole plugin for prompt quality, civic-government safety gates, and consistency with CivicWork's municipal governance conventions.

## Trigger

User invokes `/municipal-governance:skill-qa`

## Purpose

This is an internal quality review workflow for the plugin itself. It treats skills as reviewable architecture: prompts should be auditable, source-aware, state-aware, and clear about when a single AI instance is not enough.

## Inputs

- Target scope: one skill path/name, a set of skills, or the full `skills/` directory
- Optional focus: provenance, legal/compliance risk, connector assumptions, scoping, output format, or stale state references
- Optional change mode: review only, or propose edits

## Workflow

### 0. Scope the Review

Ask only what is needed:

1. **"Do you want a quick QA pass or a comprehensive review?"**
2. **"Should I review one skill, workflow skills only, domain skills only, or all skills?"**
3. **"Do you want findings only, or should I patch the issues I find?"**

If the user already specified scope, proceed without re-asking.

### 1. Load Plugin Standards

Read:
- `AGENTS.md`
- `CivicWorkPluginReference.md`
- `municipal.local.md` if the review involves state-law or municipal-context behavior
- `state-references/{STATE}.md` when checking compliance guidance

For state references, inspect freshness metadata:
- `last_verified`
- `freshness_window_days`
- `verified_against`

If metadata is missing or stale, flag that the skill should warn users before relying on state-law guidance.

### 2. Inspect the Target Skills

For each `SKILL.md`, check:

| Area | QA Questions |
|------|--------------|
| Frontmatter | Is there a clear `description` that tells Codex/Claude when to activate the skill? |
| Trigger/use guidance | Is the command or activation context explicit? |
| Scoping | Do workflow skills ask brief questions before deep work? |
| Municipal context | Does the skill load `municipal.local.md` when local context matters? |
| State law | Does the skill load and freshness-check `state-references/{STATE}.md` before compliance guidance? |
| Confidence/provenance | Do decision-relevant claims require confidence and source tags? |
| Analysis boundaries | Does high-stakes advice include a specific single-instance verification disclosure? |
| Connector behavior | Are active connectors named accurately, with fallback behavior when unavailable? |
| External actions | Do sync/send/post/link actions require user confirmation before data leaves the workspace? |
| Public-record-adjacent output | Do meeting, agenda, watcher, constituent, and briefing-book workflows avoid confidential, privileged, campaign, and private constituent material unless explicitly separated? |
| Cross-references | Are `## Skills Referenced` and `## Related Skills` present where expected? |
| Output shape | Is the template concise, skimmable, and explicit about omitting unavailable sections? |

### 3. Classify Findings

Use three severity levels:

- **P1 - Trust/Safety**: Could produce overconfident legal, fiscal, privacy, public-records, open-meetings, or external-sync behavior.
- **P2 - Reliability**: Could produce inconsistent outputs, stale guidance, missing fallbacks, or weak source traceability.
- **P3 - Maintainability**: Naming drift, missing cross-references, unclear triggers, duplicated wording, or docs mismatch.

### 4. Recommend or Apply Fixes

For review-only mode, produce findings with file paths and targeted recommendations.

For patch mode, keep edits narrow:
- Add missing provenance/confidence language
- Add state-reference freshness checks where compliance guidance appears
- Add analysis-boundary language to decision-driving workflows
- Add connector fallback notes
- Add missing related-skill references

Do not rewrite a skill's core methodology unless the user asked for a broader refactor.

## Output Format

```markdown
# Skill QA Review

**Scope**: [one skill / workflow skills / domain skills / all skills]
**Mode**: [quick / comprehensive]

## Findings

| Priority | File | Issue | Recommended Fix |
|----------|------|-------|-----------------|
| P1 | [path] | [trust/safety issue] | [specific fix] |

## Standards Checked

- Frontmatter description: [pass/fail/partial]
- Scoping step: [pass/fail/partial]
- Confidence/provenance rules: [pass/fail/partial]
- Analysis boundaries: [pass/fail/partial]
- State-reference freshness: [pass/fail/partial]
- Connector fallbacks: [pass/fail/partial]
- External action confirmation: [pass/fail/partial]
- Cross-references: [pass/fail/partial]

## Suggested Patch Scope

[Smallest safe patch that resolves P1/P2 issues.]
```

If no material issues are found, say so plainly and list any residual review gaps.

## Skills Referenced

- `municipal-code-analysis` — source/provenance and municipal code lookup standards
- `open-meetings-foia` — public-records, OMA, and communication risk checks
- `ethics-conflicts` — conflict and role-separation checks
- `policy-evaluation` — analysis-boundary and recommendation quality checks

## Related Skills

- `policy-research` — decision-focused and comprehensive research workflows need strong source handling
- `analyze-ordinance` — legal/code-conflict workflows need provenance and verification gates
- `sync-to-policyaide` — external sync actions need confirmation and data minimization
- `meeting-close-out` — institutional-memory workflows need careful distinction between records and observations
- `agenda-watcher` — watcher outputs need source traceability, fallback behavior, and careful public-record boundaries

## Notes

- This skill reviews prompt/workflow quality, not the legal correctness of state references.
- Do not assume a connector works merely because it appears in `.mcp.json`; check availability and provide fallback instructions.
- When a skill may shape an official action, prefer a specific verification disclosure over a generic disclaimer.
