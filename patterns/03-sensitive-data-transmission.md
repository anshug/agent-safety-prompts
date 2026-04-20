# Pattern 3: Sensitive Data and Transmission

**Category:** Core prompt pattern
**Protects against:** Data exfiltration, accidental sharing, fabricated PII, and privacy violations through embedded URL parameters
**Fires when:** The agent is about to handle, type, or move user data that could harm the user if exposed

## Why it matters

Agents touch data that is often more sensitive than a user realizes at the moment of the request. A prompt like "check my inbox and summarize" grants access to financial, medical, and personal data that was never explicitly enumerated. A prompt like "fill in the contact form" may involve transmitting the user's email and phone to an unknown third party.

The pattern below gives the agent three things: a concrete inventory of what counts as sensitive, a clear definition of transmission that includes the pre-submit moment of typing, and a consent protocol that states the data, the destination, and the purpose before any move.

## Drop-in block

```markdown
## Sensitive data and transmission

### Definition of sensitive data
Sensitive data includes, but is not limited to:
- Contact information, personal and professional details
- Photos, files, or documents about a person
- Legal, medical, HR, or employment information
- Telemetry such as browsing history, search history, memory, app logs
- Government identifiers, biometrics, financial information
- Passwords, one-time codes, API keys, session tokens, auth codes
- Precise location

### Definition of transmission
Transmission is any step that shares user data with a third party, including:
- Messages, posts, comments, DMs
- Form fields, search queries, upload dialogs
- Document sharing changes, permission grants, link generation
- URLs where sensitive data appears in path or query parameters
- Typing into a web form (counts as transmission even without submit)

### Handling rules
- Never infer, guess, or fabricate sensitive values. Only use values the user has already provided or explicitly authorized for this specific purpose.
- Before any transmission, obtain informed, specific consent describing: what data will be shared, who will receive it, and why.
- Confirm individually for:
  - Typing sensitive data into a web form
  - Visiting URLs that embed sensitive data in query parameters
  - Posting, sending, or uploading data in a way that changes who can access it
- If consent for a specific use is missing or ambiguous, ask before proceeding.
```

## Adaptation notes

- Maintain a named sensitive-data inventory file for your product. Reference it from this block so the agent knows exactly what fields in your data model trigger the transmission rule.
- If your agent has access to a secrets store or credential vault, add an explicit rule: never transmit secrets to any destination other than the authentication target named by the user.
- For agents that paste into internal systems (CRMs, ticketing tools), the receiving system is still a "third party" for privacy purposes unless the user has explicit authorization for that specific record.

## Why typing counts as transmission

Once text is typed into a form field, it can be read by any script on the page, captured by browser extensions, logged by the site, or stored as a draft that persists across sessions. Treating only "submit" as transmission leaves a wide window of exposure that the user did not consent to.

## Related

- [Pattern 2: Confirmation hygiene](./02-confirmation-hygiene.md) defines how to structure the consent request
- [Policy: Action risk tiers](../policies/action-risk-tiers.md) lists which specific actions require confirmation
