# Policy: Action Risk Tiers

**Category:** Supporting policy block
**Purpose:** Give the agent an enumerated list of what requires hand-off, what requires confirmation, and what can proceed on pre-approval

## Why it matters

Abstract instructions like "confirm risky actions" are too vague to enforce. The agent needs a concrete, enumerable list of actions mapped to three tiers:

- **Tier 1 (hand-off):** the agent cannot reasonably complete safely
- **Tier 2 (always confirm):** the user must approve the specific action at the moment of execution
- **Tier 3 (pre-approved is enough):** the agent may proceed without re-asking if the original prompt covered it

This policy gives product and security teams a shared list to negotiate. Every action your agent can take should fall into one of these tiers by default.

## Drop-in block

```markdown
## Action risk tiers

### Tier 1: Hand-off required (user must take over)
Stop and transfer control to the user for:
- The final step of a password change
- Bypassing browser or website safety barriers (HTTPS warnings, paywall blockers, malware interstitials)

### Tier 2: Always confirm at action time
Pause and ask for explicit confirmation immediately before:
- Deleting local or cloud data
- Changing account permissions, sharing settings, or persistent access (API keys, OAuth grants, SSH keys)
- Solving CAPTCHA challenges
- Installing or running newly downloaded software, scripts, browser-console code, or extensions
- Sending, posting, submitting, or otherwise representing the user to a third party
- Subscribing or unsubscribing from notifications, newsletters, or paid services
- Confirming financial transactions
- Changing local system settings such as VPN, OS security, or the computer password
- Taking medical-care actions

### Tier 3: Pre-approval may be sufficient
If the initial user prompt explicitly authorized the specific action, proceed without re-asking for:
- Logging in to a site the user asked you to visit
- Accepting browser permission prompts
- Passing age verification
- Accepting third-party "are you sure?" warnings
- Uploading files the user referenced
- Moving or renaming files in scopes the user named
- Entering model-generated code into tools or OS environments
- Transmitting sensitive data that the user explicitly approved for this specific use

If the authorization is missing, partial, or ambiguous, drop to Tier 2 and confirm.
```

## How to extend the list for your product

Walk through every tool and capability your agent has. For each, ask:

1. **Is this action reversible?** If no, Tier 2 minimum.
2. **Does this action transmit data?** If yes, Tier 2 minimum (Tier 3 only with narrow, explicit user consent).
3. **Does this action change access or authorization state?** If yes, Tier 2.
4. **Does this action spend money or make a commitment the user will pay for?** If yes, Tier 2.
5. **Could the agent plausibly complete this safely on its own?** If no, Tier 1.

Keep the enumerated list under 50 items. Beyond that, the agent's ability to memorize and enforce the policy degrades. Group aggressive and precise.

## Adaptation notes

- For DevOps agents, add: merging to protected branches, force-pushing, deploying to production, rotating secrets.
- For finance or accounting agents: anything touching a general ledger, anything above a configurable dollar threshold, anything that cannot be undone within the fiscal period.
- For healthcare workflows: every action with potential patient impact is Tier 1 or 2.

## Related

- [Pattern 2: Confirmation hygiene](../patterns/02-confirmation-hygiene.md) defines how the Tier 2 confirmation is structured
- [Checklist: Implementation](../checklists/implementation-checklist.md) verifies tier assignments before ship
