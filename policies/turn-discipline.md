# Policy: Turn Discipline

**Category:** Supporting policy block
**Purpose:** Keep the agent's loop predictable, auditable, and resistant to accidental resource leaks or state pollution

## Why it matters

Agent loops that behave unpredictably at turn boundaries are hard to audit and hard to make safe. Common failures include:

- Final-answer messages that contain clarifying questions, making it impossible to tell if the agent is done
- Runtime handles (browsers, contexts, pages) closed prematurely mid-task, losing state
- Screenshots written to temporary files to be passed back to the model, polluting the filesystem
- Tool-call-and-final-answer in the same turn, creating ambiguity about what the user should respond to

This policy block gives the agent a clean state machine: take actions, return observations, mark final answers only when truly done, and never leak resources.

## Drop-in block

```markdown
## Turn and termination rules

- Complete the user's task as fully as safe progress allows.
- Only mark a turn as a final answer when:
  1. No additional tool calls are needed in the same turn, AND
  2. Either the task is complete, or you are handing off to the user for a Tier 1 or Tier 2 confirmation.
- If the task is blocked on clarification, use an explicit `ask_user` style tool call rather than posing a question inside a final-answer message.
- Do not close browser, context, or runtime handles unless the user explicitly asks you to.
- Never write screenshots or image data to temporary files solely to pass them back to yourself. Keep image data in memory and return it through the designated image-output helper.
```

## Why separate "ask_user" from final-answer

A final-answer message says "the conversation can end here." A question says "I need input to continue." Conflating them means:

- The user can't tell whether to respond or let the agent run
- Metrics like "turns to completion" become unreliable
- Automated evaluators treat the session as done when it isn't
- Logs lose the clean separation between task progress and clarification

The fix is a dedicated `ask_user` tool that the agent calls when blocked. The calling application renders it as a prompt to the user, captures the response, and feeds it back as tool output. The model never poses clarifying questions in final-answer text.

## Why not close handles

Most code-execution harnesses keep a browser, context, or page alive across tool calls. Closing them prematurely:

- Loses authentication state the user just went through
- Forces re-navigation and re-loading, burning tokens
- Can trigger the site's "logged out" flow, confusing the next turn

Let the outer harness own the lifecycle. The agent only closes resources when the user explicitly asks.

## Why not write images to disk

Writing screenshots to temporary files just to pass them back to the model:

- Leaves forensic artifacts on the filesystem
- Can clash with real user files if paths are not carefully scoped
- Adds latency
- Invites file-path-based prompt injection

Keep image data in memory. Use the harness's designated image-output helper (in the OpenAI Computer Use guide this is `display(base64_string)`; your harness may name it differently).

## Adaptation notes

- If your harness supports structured phase markers (like `commentary` vs `final_answer`), lean on them to disambiguate turn state.
- For long-running tasks, add a "progress update" tool that's distinct from both `ask_user` and final-answer, so the agent can narrate without ending the turn.
- For batch-mode agents, replace `ask_user` with a structured "escalation record" that pauses the task and routes it to a queue.

## Related

- [Policy: Environment isolation](./environment-isolation.md) sets up the runtime this policy keeps clean
- [Checklist: Implementation](../checklists/implementation-checklist.md) includes turn-discipline verification
