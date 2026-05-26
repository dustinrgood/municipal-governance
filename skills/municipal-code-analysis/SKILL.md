---
description: >
  This skill should be used when the user needs to analyze, interpret,
  cross-reference, or understand municipal code provisions. Triggers include
  any mention of ordinances, municipal code sections, zoning code, code
  amendments, code compliance, or when evaluating how a proposed action
  relates to existing municipal regulations.
---

# Municipal Code Analysis

## Overview

Municipal codes are the comprehensive body of local law governing a municipality. They are organized by title, chapter, and section, and include everything from zoning regulations to business licensing to public health requirements.

## Code Structure

Most municipal codes follow a hierarchical structure:

- **Title**: Broad subject area (e.g., Title 19 - Zoning)
- **Chapter**: Subdivision of title (e.g., Chapter 19.07 - Residential Districts)
- **Section**: Specific provision (e.g., 19.07.050 - Permitted Uses)

Common title organization:
1. General Provisions / Administration
2. Elections
3. Revenue and Finance
4. Business Licenses and Regulations
5. Health and Safety
6. Animals
7. Public Peace, Morals and Welfare
8. Vehicles and Traffic
9. Streets, Sidewalks and Public Places
10. Parks and Recreation
11. Public Utilities
12. Buildings and Construction
13. Subdivision Regulations
14. Zoning

## Analysis Framework

When analyzing code provisions:

### 1. Identify the Operative Language

- **"Shall"** = mandatory requirement
- **"May"** = permissive/discretionary
- **"Should"** = advisory (rare in code, more common in plans)
- **"Is prohibited"** = absolute prohibition
- **"Upon finding that..."** = conditions precedent

### 2. Check Definitions

- Always reference the definitions section of the relevant title
- Terms may have specific legal meanings that differ from common usage
- Look for both title-specific definitions AND general code definitions
- Note when terms are undefined (may require interpretation)

### 3. Cross-Reference Related Provisions

Code sections rarely stand alone. Always check for:

- Sections explicitly referenced in the provision
- Exceptions and exemptions
- General provisions that apply code-wide
- Penalty/enforcement provisions
- Conflict resolution provisions (which section controls when provisions conflict)
- Related state law requirements

### 4. Consider the Hierarchy of Authority

1. **Federal Constitution** - supreme law
2. **Federal statutes and regulations** - preempt conflicting state/local law
3. **State Constitution** - may grant or limit local authority
4. **State statutes** - often preempt local ordinances on specific topics
5. **Local charter/constitution** (if home rule)
6. **Local ordinances** - municipal code
7. **Administrative rules/regulations** - implement ordinances

### 5. Check Amendment History

- Recent amendments may signal policy direction
- Pending amendments may affect analysis
- Repealed provisions may be relevant for grandfathered uses
- Effective dates matter for applicability

## Home Rule vs. Dillon's Rule

**Home Rule Municipalities**:
- Broad authority to legislate on local matters
- Can act unless specifically prohibited by state law
- Greater flexibility in code drafting
- May have charter that functions as local constitution

**Dillon's Rule (Non-Home Rule)**:
- Limited to powers expressly granted by state
- Must point to specific enabling authority
- Ambiguities resolved against municipal authority
- More constrained in code drafting

## Common Code Analysis Tasks

### Zoning Compliance Check
1. Identify the zoning district
2. Find permitted uses for that district
3. Check if use requires special/conditional use permit
4. Review dimensional requirements (setbacks, height, FAR)
5. Check parking requirements
6. Identify any overlay districts
7. Review nonconforming use provisions if applicable

### Ordinance Amendment Analysis
1. Identify all sections being amended
2. Compare old and new language (redline)
3. Check for internal consistency
4. Verify cross-references remain valid
5. Assess conflicts with other code sections
6. Consider state preemption issues
7. Review procedural requirements for adoption

### Enforcement Analysis
1. Locate violation provisions
2. Identify penalty structure
3. Check enforcement authority
4. Review appeal/hearing procedures
5. Assess statute of limitations
6. Consider due process requirements

## Related Skills

- `land-use-zoning` — zoning code analysis, variance findings, development review
- `public-finance` — levy ordinances, budget-related code provisions, TIF compliance
- `ethics-conflicts` — local ethics ordinance analysis, prohibited transaction provisions
- `open-meetings-foia` — OMA compliance requirements codified in local rules

## Using Connected Tools

### MunicipalMCP Tool Reference

The `municipal-code` connection provides access to municipal codes via the Municode digital library. Use values from `municipal.local.md` for `municipality_name` and `state_abbr` in all tool calls.

| Tool | Purpose | Key Parameters |
|------|---------|---------------|
| `search_municipal_codes` | Full-text search of code provisions | `municipality_name`, `state_abbr`, `search_query`, `page_size` (default 10), `titles_only` (boolean) |
| `get_code_section` | Retrieve full text of a specific code section | `municipality_name`, `state_abbr`, `node_id` (required) |
| `get_code_structure` | Browse table of contents / navigate code hierarchy | `municipality_name`, `state_abbr`, `node_id` (optional — omit for top-level TOC) |
| `get_municipality_info` | Confirm municipality is in Municode and see available products | `municipality_name`, `state_abbr` |
| `get_municipality_url` | Get URL to the municipality's Municode library page | `municipality_name`, `state_abbr` |
| `list_municipalities` | List all Municode municipalities in a state | `state_abbr` |
| `get_states_info` | Get state-level information | `state_abbr` |

### Common Workflows

**Search-then-Read** (most common pattern):
1. Use `search_municipal_codes` with a targeted query to find relevant provisions
2. Review search result snippets and note the `node_id` of relevant sections
3. Use `get_code_section` with the `node_id` to retrieve full section text
4. Repeat for cross-referenced sections

**Browse-then-Read** (when exploring unfamiliar code areas):
1. Use `get_code_structure` without a `node_id` to get the top-level table of contents
2. Identify the relevant title (e.g., "Zoning," "Finance," "Administration")
3. Use `get_code_structure` with the title's `node_id` to drill into chapters
4. Continue drilling until you find the target section
5. Use `get_code_section` to retrieve the full text

**Validation** (confirm municipality availability):
1. Use `get_municipality_info` to verify the municipality is in the Municode system
2. If the name doesn't match, use `list_municipalities` to find the exact name Municode uses
3. Use `get_municipality_url` to provide a direct link for manual verification

### Search Tips

- Use `titles_only=true` to search headings only — faster, useful for finding the right chapter or article
- Use precise legal terms (e.g., "conditional use permit" rather than "special permission")
- Set `page_size=20` for broad scans when you need to survey a topic area
- Search for defined terms (e.g., "as defined in") to find the definitions section
- Try multiple search terms — municipal codes vary widely in terminology

### Common Pitfalls

- **`node_id` is required for `get_code_section`**: You cannot retrieve section text without it. Get the `node_id` from search results or `get_code_structure`.
- **Search results return snippets, not full text**: Always follow up with `get_code_section` for the complete provision.
- **Municipality name must match Municode's records**: "City of Springfield" vs. "Springfield" — if a lookup fails, use `list_municipalities` to find the exact name.
- **Codes may not reflect recent amendments**: Online codes can lag behind adopted ordinances. Note the "current through" date when citing code provisions.
- **Not all municipalities use Municode**: If `get_municipality_info` returns no results, the municipality may use a different code publisher (American Legal, General Code, etc.).

### When Tools Are Unavailable

When the `municipal-code` connection is not available:
- Note the gap explicitly in your analysis
- Work from uploaded code documents, screenshots, or user-provided section text
- Suggest the user verify current code provisions via their municipality's code website
- Cite section numbers based on available information and flag them for verification

**Optional and planned connectors** (plugin works without these):
- `document-management` (optional/configured when authenticated) — staff reports and supporting materials
- `agenda-management` (planned; not yet implemented in this plugin) — legislation history

## Municipal Configuration

Check `municipal.local.md` for:
- Municipality-specific code provider and URL
- Key code references (zoning, subdivision, etc.)
- Active TIF districts and special districts
- State-specific legal framework
- Local procedural requirements

## Procedural Requirements Checklist

When reviewing any action against the code, verify:
- [ ] Public hearing requirements
- [ ] Notice requirements (mail, posting, publication)
- [ ] Voting requirements (simple majority, supermajority)
- [ ] Effective date rules
- [ ] Recording/filing requirements

## Output Template

When producing a code compliance review, use this structure:

```markdown
# Code Compliance Review: [Subject]

## Summary
[Key compliance findings in 2-3 sentences]

## Subject of Review
[Description of what's being reviewed]

## Source and Confidence Notes
- Code source: [municipal-code / uploaded code excerpt / municipal website / user-provided citation]
- Code currency: [current-through date if available / unknown]
- State reference: [state-reference/XX last_verified YYYY-MM-DD / stale / unavailable]
- Claims requiring verification: [short list of Medium/Low confidence code or legal conclusions]

## Relevant Code Sections

### Primary Sections
| Section | Title | Relevance | Confidence / Provenance |
|---------|-------|-----------|-------------------------|
| [X.XX.XXX] | [Title] | [Why relevant] | [High/Med/Low; source] |

### Supporting Sections
| Section | Title | Relevance | Confidence / Provenance |
|---------|-------|-----------|-------------------------|
| [X.XX.XXX] | [Title] | [Why relevant] | [High/Med/Low; source] |

### Definitions (from [Section])
- **[Term 1]**: [Definition]
- **[Term 2]**: [Definition]

## Compliance Analysis

### Authority
- **Legal basis**: [Code section authorizing action]
- **Limitations**: [Any restrictions on authority]
- **Home rule considerations**: [If applicable]

### Requirements
| Requirement | Code Section | Status | Notes | Confidence / Provenance |
|-------------|--------------|--------|-------|-------------------------|
| [Requirement 1] | [Section] | ✅/❌/⚠️ | [Details] | [High/Med/Low; source] |
| [Requirement 2] | [Section] | ✅/❌/⚠️ | [Details] | [High/Med/Low; source] |

### Procedural Requirements
- [ ] [Requirement 1] - [Section reference]
- [ ] [Requirement 2] - [Section reference]

## Issues Identified

### Conflicts
| Issue | Sections | Description | Resolution |
|-------|----------|-------------|------------|
| [Issue 1] | [X] vs [Y] | [Description] | [How to resolve] |

### Gaps
- [Gap 1]: [Description]

### Ambiguities
- [Section X]: [Ambiguity description]

## Cross-Reference Check

### References Updated
| Section | Reference | Status |
|---------|-----------|--------|
| [X.XX] | [What it references] | ✅/Needs update |

### Impacted Sections
| Section | Impact | Action Needed |
|---------|--------|---------------|
| [X.XX] | [How affected] | [Update needed] |

## Compliance Determination

**Overall Status**: [Compliant / Non-Compliant / Requires Modification] *(Confidence: [High/Medium/Low]; provenance: [source])*

**Conditions for Compliance**:
1. [Condition 1]
2. [Condition 2]

## Recommendations

### Required Actions
- [Action 1]

### Suggested Improvements
- [Suggestion 1]

## Caveats
- This analysis is based on [code version/date]
- Complex legal questions should be referred to the municipal attorney
```

## Quality Standards

- Cite specific section numbers
- Quote operative language when important
- Include confidence and provenance for authority, conflict, compliance, and procedural conclusions
- Note version/date of code reviewed
- Identify ambiguities honestly
- Flag items requiring attorney review
- Don't overstate certainty on legal questions

## Important Caveats

- Code analysis supports policy deliberation but does not constitute legal advice
- Complex legal questions should be referred to the municipal attorney
- Always verify current code version (codes are frequently amended)
- Online codes may have lag time from recent amendments
