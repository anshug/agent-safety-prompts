# Pattern 2: Confirmation Hygiene

**Category:** Core prompt pattern
**Protects against:** Premature confirmation prompts that train users to click-through, bundled vague asks, and sensitive data entered before consent
**Fires when:** The agent is about to take an action that crosses a defined risk boundary

## Why it matters

Two failure modes are common in agent UX:

1. **Ask-early, ask-often.** The agent stops at the start of every task and asks "are you sure?" The user learns to auto-approve. Real risky actions slip through.
2. **Vague bundled asks.** The agent batches "delete files, rename folder, send email, update permissions" into one confirmation. The user can't meaningfully consent.

The pattern below fixes both. It defers confirmation until the agent is actually at the risk boundary, requires confirmations to be specific and mechanism-aware, and treats typing sensitive data as transmission (you cannot un-type a password into a field).

## Drop-in block

```markdown
## Confirmation hygiene

### Timing
- Do not ask for confirmation at the start of a task when safe progress is still possible.
- Complete as much of the task as you can, then pause immediately before the next action that crosses a risk boundary.
- Exception: for sensitive data, confirm BEFORE typing or submitting. Typing sensitive data into a form counts as transmission and cannot be undone.

### Batching
- You may group multiple imminent, well-defined risky actions into a single confirmation.
- Do NOT bundle vague or future-looking steps. Each confirmation must be specific enough that the user knows exactly what will happen next.

### Content of the confirmation
Every confirmation request must state:
1. The exact action you are about to take
2. The specific data, account, or asset affected
3. The risk or irreversibility involved
4. How the user's response will be applied
```

## Adaptation notes

- If your product supports "confirmation modes" (strict, balanced, autopilot), map them to how aggressively the agent should batch versus ask. Even in autopilot, keep the "state the risk" requirement.
- For agents that run unattended in batch mode, replace confirmation with structured logging of every Tier 2 action and require a human-approved run list before execution.
- Build confirmation templates into your UI layer so the agent cannot skip the 4-item content requirement.

## Related

- [Policy: Action risk tiers](../policies/action-risk-tiers.md) defines what crosses a risk boundary
- [Pattern 3: Sensitive data and transmission](./03-sensitive-data-transmission.md) covers the typing-as-transmission exception
