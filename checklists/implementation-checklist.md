# Implementation Checklist

Use this checklist before shipping any agent that uses Computer Use, a custom harness (Playwright, Selenium, MCP), or a code-execution harness. Every item should be verifiable by inspection of the runtime, the prompt, or the logs.

## Environment

- [ ] Agent runs in an isolated browser context, container, or VM
- [ ] Runtime receives an empty environment variable set (host secrets not inherited)
- [ ] Browser extensions disabled; local filesystem access disabled where not needed
- [ ] Viewport set to 1440x900, 1600x900, or 1280x720 (or matches model's trained range)
- [ ] Allow list of domains, apps, and actions is in place; default-deny otherwise
- [ ] Allow list is versioned in source control, not just in the prompt
- [ ] Outbound network from the runtime is restricted to the allow list
- [ ] Snapshot and rollback are possible per session for VM-based setups

## Policy (system prompt or AGENTS.md)

- [ ] Pattern 1 (Trust boundary) present in the agent's instructions
- [ ] Pattern 2 (Confirmation hygiene) present
- [ ] Pattern 3 (Sensitive data and transmission) present, with product-specific data inventory referenced
- [ ] Pattern 4 (Prompt injection handling) present, with examples from the product's threat landscape
- [ ] Action risk tiers (Tier 1, 2, 3) encoded as concrete lists of actions the agent can actually take
- [ ] Turn discipline rules present
- [ ] Visual input hygiene rules present for any agent that handles screenshots or images

## Runtime

- [ ] `ask_user` or equivalent tool exposed for clarification; agent never asks questions inside final-answer messages
- [ ] Screenshots captured at detail "original"; coordinate remapping in place if downscaling
- [ ] Image helpers (e.g., `display()`) return images directly; no intermediate file writes
- [ ] Every tool call, navigation, and action attempt is logged with timestamp, session ID, and outcome
- [ ] Logs are immutable for audit window (at least 30 days recommended)
- [ ] Final-answer markers are gated on "no pending tool calls in the same turn"
- [ ] Resource handles (browser, context, page, runtime) are owned by the harness, not the model

## Human in the loop

- [ ] Hand-off path (Tier 1) tested: password-change final step, HTTPS warning bypass
- [ ] Tier 2 confirmation UI or message template ships and is reviewed for the 4-item content requirement (action, asset, risk, application)
- [ ] Kill switch exists that halts the agent and surfaces partial progress to the user
- [ ] Escalation path for suspected prompt injection is defined and tested
- [ ] On-call or review owner is named for the agent's operation

## Data handling

- [ ] Sensitive data inventory exists for the product, and the agent's prompt references it
- [ ] Secrets and credentials pass through a vault; they are never typed into agent-visible fields outside the authentication target
- [ ] DLP or redaction layer exists between the agent and any external send path, where required by your regulatory posture
- [ ] Short-term memory and long-term memory are documented; injected instructions cannot enter persistent memory

## Evaluation

- [ ] Red-team prompt set covering each of the four patterns has been run against the agent
- [ ] Injection corpus includes page-content, document, image-text, and tool-output injections
- [ ] Recovery behavior tested: unexpected page state, authentication failure, CAPTCHA
- [ ] Metrics captured for each run: turn count, time to complete, confirmation count, injection-detected events

## Documentation

- [ ] README or ops doc states what the agent is allowed to do and where to find the policy
- [ ] Users or administrators can see the agent's action log for their own sessions
- [ ] Incident response path is documented: who to contact, how to revoke agent access, how to roll back changes

## Ship / no-ship decision

If any item in this checklist is unchecked, you are shipping at elevated risk. Decide explicitly whether each open item is accepted, mitigated, or blocking, and record that decision. Do not ship with silent gaps.
