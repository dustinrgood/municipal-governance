---
description: >
  This skill should be used when the user needs to draft a professional
  response to constituent communications following municipal communication
  standards. Triggers include any mention of constituent email, citizen
  inquiry, public complaint, responding to a resident, drafting a reply
  to a community member, or handling correspondence from the public.
---

# Constituent Response

Draft professional responses to constituent communications following municipal communication standards.

## Trigger

User invokes `/municipal-governance:constituent-response`

## Inputs

- Original constituent message (email, letter, voicemail transcript)
- Desired tone (formal, warm, direct)
- Response type (acknowledgment, substantive response, follow-up)
- Any specific points to address
- Responder role (elected official, staff)

## Workflow

### 1. Analyze the Incoming Message

Identify:
- **Topic(s)**: What issue(s) is the constituent raising?
- **Tone**: Constructive, frustrated, angry, appreciative?
- **Ask**: What do they want? (Information, action, acknowledgment, complaint resolution)
- **Urgency**: Time-sensitive? Ongoing issue?
- **History**: Is this a follow-up to prior communication?

### 2. Determine Response Approach

**Acknowledgment Only** (when full response will follow):
- Confirm receipt
- Set expectations for response timeline
- Provide interim information if available

**Substantive Response**:
- Address specific questions/concerns
- Provide requested information
- Explain policy/position
- Describe next steps

**Referral Response**:
- Explain correct contact/department
- Provide contact information
- Offer to forward (if appropriate)

### 3. Gather Information

Using available tools when authenticated or installed:
- `municipal-code`: Relevant ordinances
- `document-management`: Prior correspondence, policies, or approved records when Box access is authenticated
- `agenda-management`: Related council actions, if a future agenda connector is installed
- Web search: General information if needed

If connectors are unavailable, work from the message, uploaded materials, official municipal sources, or user-provided context. Do not imply prior correspondence, policies, or council actions were reviewed unless they were actually retrieved.

### 4. Draft Response

Structure based on best practices:

**Opening**
- Thank them for contacting
- Acknowledge their specific concern
- Match tone appropriately

**Substance**
- Address their points directly
- Provide factual information
- Explain "why" not just "what"
- Be honest about limitations

**Path Forward**
- What happens next
- Who they can contact
- How to stay informed
- Invitation for follow-up

**Closing**
- Reaffirm appreciation
- Provide contact info
- Professional sign-off

### 5. Tone Calibration

**For frustrated/angry constituents**:
- Acknowledge frustration without defensiveness
- Focus on solutions
- Avoid bureaucratic language
- Be direct about what can/cannot be done

**For appreciative constituents**:
- Accept thanks graciously
- Share credit appropriately
- Reinforce civic engagement

**For routine inquiries**:
- Be helpful and efficient
- Provide complete information
- Anticipate follow-up questions

## Output Format

```markdown
# Draft Constituent Response

**To**: [Constituent name]
**From**: [Your name/title]
**Re**: [Subject - their topic]
**Date**: [Date]

---

[RESPONSE TEXT]

---

## Response Notes

**Key points addressed**:
- [Point 1]
- [Point 2]

**Information sources**:
- [Source used]

**Follow-up needed**:
- [ ] [Any follow-up action]

**Routing**:
- CC: [If anyone should be copied]
- Forward to: [If referral needed]
```

## Response Templates

### Acknowledgment Template

```
Dear [Name],

Thank you for contacting [me/the City] about [topic]. I appreciate you
taking the time to share your [concerns/thoughts/questions].

[Brief statement showing you understood their message]

I wanted to let you know that I've received your message and am looking
into this matter. You can expect a more detailed response by [timeframe].

In the meantime, if you have additional information to share or questions,
please don't hesitate to reach out.

Thank you again for your engagement in our community.

[Signature]
```

### Information Request Template

```
Dear [Name],

Thank you for your inquiry about [topic].

[Substantive response to their question]

[Additional context if helpful]

[Resources or next steps]

If you have additional questions, please feel free to contact [me/department]
at [contact info].

Thank you for your interest in [topic/city government].

[Signature]
```

### Complaint Response Template

```
Dear [Name],

Thank you for contacting [me/the City] about [issue]. I understand your
frustration with [acknowledge specific concern], and I appreciate you
bringing this to [my/our] attention.

[Explanation of situation - facts, not excuses]

[What is being done / What can be done / What the limitations are]

[Specific next steps with timeline if possible]

I want you to know that [expression of commitment to addressing concerns /
to quality service / etc.]. Please don't hesitate to reach out if you have
additional questions or if the situation continues.

[Signature]
```

### Referral Template

```
Dear [Name],

Thank you for contacting [me/the City] about [topic].

While I appreciate you reaching out, this matter is handled by [correct
department/person]. I want to make sure your concern gets to the right
people who can help you.

Please contact:
[Name/Department]
[Phone]
[Email]
[Office hours if relevant]

[Optional: I've forwarded your message to them and asked them to follow up
with you directly.]

If you have any difficulty reaching them or if I can help in any other way,
please let me know.

[Signature]
```

### Policy Explanation Template

```
Dear [Name],

Thank you for your question about [policy/topic].

[Explanation of the policy]

[Why this policy exists - the rationale]

[How it applies to their situation]

[Any options or exceptions they should know about]

I hope this helps clarify [the policy/situation]. If you have additional
questions or would like to discuss this further, please [contact info /
attend council meeting / etc.].

[Signature]
```

## Skills Referenced

- `council-communication` — document formats, tone, writing standards
- `open-meetings-foia` — route FOIA requests to FOIA officer rather than responding substantively

## Notes

- **Response time benchmarks**: Initial acknowledgment within **24-48 hours**; substantive response within **3-5 business days**. For part-time council members without dedicated staff, route constituent service requests through the city manager/administrator per council-manager government protocol.
- **Available CRM tools for small municipalities**: CivicPlus (integrates with municipal websites), CivicTrack (purpose-built for elected officials), Polco (NLC partnership for community engagement). Even basic systems reduce response tracking burden for part-time officials.
- Always match formality to the constituent's communication
- Never be defensive or dismissive
- Acknowledge valid frustrations even when you can't resolve them
- Be honest about what you can and cannot do
- Avoid promising what you can't deliver
- Use plain language, not bureaucratic jargon
- Proofread carefully - this represents the municipality
- Flag responses with high public visibility or community sensitivity for review
- This skill produces a draft only. Never transmit a response through a connected email, messaging, CRM, or communication connector without showing the final text and getting explicit user confirmation in the current session.
- Treat constituent communications and draft replies as public-record-adjacent. Avoid unnecessary private constituent details, closed-session material, privileged legal advice, campaign strategy, or electioneering content in official-government responses.
- **FOIA requests**: If a constituent message is actually a public records request (FOIA), do not attempt a substantive response. Instead, acknowledge receipt and route to the designated FOIA officer listed in `municipal.local.md`. FOIA requests have legal deadlines and procedures that must be followed.
- **Threatening or abusive communications**: If a message contains threats of violence, route to law enforcement immediately. For abusive but non-threatening messages, respond briefly and professionally, acknowledge any legitimate underlying concern, and do not engage with the hostile tone. If harassment is persistent, consult the municipal attorney about appropriate steps.

## Related Skills

- `policy-research` — if the constituent's question requires deeper research to answer well
- `analyze-ordinance` — if the inquiry relates to a specific ordinance or code provision
