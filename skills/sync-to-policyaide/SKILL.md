---
description: Sync official profile, municipality context, and standing document summaries to PolicyAide for personalized multi-agent research. Use when the official wants to connect their CivicWork Plugin data with the PolicyAide research platform.
---

# Sync to PolicyAide

Syncs your official profile, municipality context, and standing document summaries from the CivicWork Plugin to PolicyAide (app.civicwork.ai). This enables personalized, context-rich multi-agent research that understands your city, your priorities, and your key governance documents.

## Trigger

`/municipal-governance:sync-to-policyaide`

## Prerequisites

- `municipal.local.md` must be configured (run `setup-municipality` first)
- `official.local.md` should be configured (run `setup-official` first, or this skill will sync municipality data only)

## Privacy and Confirmation Rules

PolicyAide sync sends local profile and municipal context outside the workspace. Before any link or sync request leaves the machine:
- Explain exactly what endpoint will receive data
- Show a concise summary of the payload fields to be sent
- Omit bracket placeholders, private constituent details, staff-only notes, privileged information, and documents the user has not approved for sync
- Keep government-role context separate from campaign or electioneering material
- Ask for explicit user confirmation before POSTing

`campaignPlatform` may include public campaign positions if the user confirms they want that context included. Do not sync campaign strategy, donor/voter data, persuasion plans, or election operations.

## Workflow

### Step 1: Check Configuration

Read `municipal.local.md` and `official.local.md` (if it exists). Verify that at minimum, municipality name and state are populated.

If `official.local.md` doesn't exist or has bracket placeholders for name/role:
> "I notice you haven't set up your official profile yet. Would you like to run that first? It captures your priorities and platform, which makes PolicyAide research more personalized.
>
> (You can also sync just your municipality data now and add the personal profile later.)"

### Step 2: Check Link Status

Read `official.local.md` and check the **PolicyAide Link** section:
- If **Link Token** has a value (not `[not yet linked]`), proceed to Step 4.
- If not linked, proceed to Step 3.

### Step 3: Link to PolicyAide

Guide the user through the one-time linking process:

> "To sync with PolicyAide, we need to link your plugin to your PolicyAide account. This is a one-time setup:
>
> 1. Go to **app.civicwork.ai/dashboard**
> 2. Find the **Plugin Link** card
> 3. Click **Generate Link Code** — you'll get a 6-character code
> 4. Tell me the code and I'll complete the link"

When the user provides the code:

1. Confirm that the user wants to send the link code to PolicyAide:
   > "I'll send this one-time link code to `https://app.civicwork.ai/api/plugin/link` to connect this plugin to your PolicyAide account. No profile or municipal context is sent in this step. Should I proceed?"

2. Use `web_fetch` to POST to `https://app.civicwork.ai/api/plugin/link`:
   ```
   Method: POST
   Headers: Content-Type: application/json
   Body: { "linkCode": "[USER'S CODE]" }
   ```

3. If successful (response contains `pluginToken`):
   - Store the `pluginToken` in `official.local.md` under PolicyAide Link section
   - Update `Link Token`, `Linked At`, and `PolicyAide User ID` fields
   - Confirm: "Plugin linked successfully! Your data will now sync with PolicyAide."

4. If failed:
   - Code expired: "That code has expired. Please generate a new one on your dashboard."
   - Invalid code: "That code wasn't recognized. Please double-check and try again."
   - Already used: "That code has already been used. Generate a new one if needed."

### Step 4: Build Sync Payload

Assemble the data to sync from local files.

Data minimization rules:
- Omit fields that still contain bracket placeholders
- Omit private constituent case details, staff-only notes, privileged legal material, and confidential records
- Include `campaignPlatform` only when it reflects public campaign positions and the user explicitly approves including it
- Send standing document summaries and references, not full documents, unless a future workflow asks for full-document sync and gets separate confirmation

**Profile Sync payload** (from `official.local.md` + `municipal.local.md`):
```json
{
  "handoffType": "profile_sync",
  "pluginVersion": "0.5.0",
  "payload": {
    "officialProfile": {
      "name": "[from official.local.md]",
      "role": "[from official.local.md]",
      "email": "[from official.local.md]",
      "yearsInOffice": "[from official.local.md]",
      "termEnd": "[from official.local.md]",
      "campaignPlatform": "[from official.local.md]",
      "policyPriorities": ["[from official.local.md]"],
      "pastPolicyWork": "[from official.local.md]",
      "preferredTone": "[from official.local.md]",
      "primaryAudience": "[from official.local.md]"
    },
    "municipalContext": {
      "name": "[from municipal.local.md Municipality > Name]",
      "state": "[from municipal.local.md Municipality > State, as 2-letter code]",
      "population": "[from municipal.local.md Municipality > Population]",
      "homeRule": "[from municipal.local.md Municipality > Home Rule, as boolean]",
      "governmentType": "[from municipal.local.md Municipality > Government Type]",
      "governingBodySize": "[count from Council Structure > Members table]",
      "meetingSchedule": "[from municipal.local.md Council Structure > Meeting Schedule]",
      "fiscalYear": "[from municipal.local.md Budget Context > Fiscal Year]",
      "generalFundSize": "[from municipal.local.md Budget Context > General Fund Size]",
      "policyPriorities": ["[from municipal.local.md Policy Priorities list]"]
    }
  },
  "municipalityContext": {
    "name": "[municipality name]",
    "state": "[state code]"
  }
}
```

**Standing Documents payload** (if `standing-documents.md` exists in workspace or plugin directory):
```json
{
  "handoffType": "standing_docs",
  "pluginVersion": "0.5.0",
  "payload": {
    "documents": [
      {
        "type": "strategic_plan",
        "title": "[from standing-documents.md]",
        "url": "[from standing-documents.md]",
        "discoveredAt": "[from standing-documents.md]",
        "keyGoals": ["[from standing-documents.md]"],
        "summary": "[from standing-documents.md]",
        "relevanceToCurrentPriorities": "[from standing-documents.md]"
      }
    ]
  }
}
```

### Step 5: Send Sync

For each payload (profile_sync, then standing_docs if available):

1. Show a concise payload summary before sending:
   - Endpoint: `https://app.civicwork.ai/api/plugin/handoff`
   - Handoff types: [profile_sync / standing_docs]
   - Official fields included: [list]
   - Municipality fields included: [list]
   - Standing documents included: [titles only]
   - Sensitive fields excluded: [list]

2. Ask for explicit confirmation:
   > "Do you want me to send this payload to PolicyAide now?"

3. If the user does not confirm, stop and keep the payload local.

4. If the user confirms, use `web_fetch` to POST to `https://app.civicwork.ai/api/plugin/handoff`:
   ```
   Method: POST
   Headers:
     Content-Type: application/json
     X-Plugin-Token: [stored token from official.local.md]
   Body: [payload from Step 4]
   ```

5. Check response:
   - Success: Note the `handoffId` for status tracking
   - Failure: Report error and suggest retry

### Step 6: Report Results

Summarize what was synced:

> "Sync complete! Here's what was sent to PolicyAide:
>
> **Profile Data**:
> - Official: [name], [role]
> - Municipality: [city], [state] (pop. [population])
> - Priorities: [list top 3]
> - Platform: [summary snippet]
>
> **Standing Documents**: [list synced docs or "none — run setup-official to discover these"]
>
> **What this means**: PolicyAide research sessions will now include your municipal context, priorities, and standing document references automatically. When you escalate a policy topic from the plugin, it'll skip expensive web searches and use this pre-loaded context instead.
>
> Run this again anytime your profile or priorities change."

## Notes

- The plugin token is stored locally in `official.local.md` — it never leaves your machine except in API headers
- Standing documents are sent as summaries, not full documents
- PolicyAide respects the `useForAiRecommendations` privacy setting — if disabled in PolicyAide, profile data won't be used for AI personalization even if synced
- If `official.local.md` doesn't exist, only municipality context (from `municipal.local.md`) is synced
- Omit any fields that still have bracket placeholders — don't send placeholder text
- Treat synced official-profile and municipal-context material as potentially public-record-adjacent. Do not include confidential constituent casework, privileged legal advice, closed-session material, or campaign operations.
- Never sync or link without explicit user confirmation in the current session

## Skills Referenced

- `policy-evaluation` — structures context for downstream policy research and deliberation
- `open-meetings-foia` — public-records and confidentiality risk checks
- `ethics-conflicts` — government-role and campaign-role separation
- `council-communication` — concise official-profile and municipal-context summaries

## Related Skills

- `policy-research` — can escalate decision-focused research to PolicyAide after confirmation
- `meeting-close-out` — produces follow-up memory that may inform future profile/context updates
- `skill-qa` — reviews external-sync confirmation and data minimization gates
