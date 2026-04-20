[![License: CC BY 4.0](https://img.shields.io/badge/License-CC_BY_4.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)

# Agent Safety Prompts

**Drop-in prompt patterns, policy blocks, and checklists for building safe AI agents.**

A curated reference for teams building agents that use Computer Use, Playwright, Selenium, MCP servers, or code-execution harnesses. Every block is written to be pasted directly into a system prompt, AGENTS.md, or agent policy file.

A curated, drop-in set of prompt blocks and policy templates for builders who run agents that touch real systems: browsers, VMs, code-execution harnesses, MCP servers, and operating-system level tools. Every block here is written to be pasted directly into a system prompt, `AGENTS.md`, or agent policy file.

## Who this is for

- Developers shipping agents that use Computer Use, custom Playwright or Selenium harnesses, MCP tool servers, or code-execution runtimes
- Security teams reviewing agent integrations before they reach production
- CISOs and security architects defining guardrails for AI-assisted development inside their organization
- Open-source maintainers adding safety scaffolding to agent projects

## Quickstart

1. Clone the repo.
2. Copy [`AGENTS.md`](./AGENTS.md) to the root of your agent repo, or paste its contents into your system prompt.
3. Adapt the allow lists, sensitive data inventory, and sign-off names to your environment.
4. Walk through [`checklists/implementation-checklist.md`](./checklists/implementation-checklist.md) before you ship.

If you want smaller blocks instead of the consolidated template, pick the individual pattern and policy files below and compose your own.

## Repository layout

```
.
├── README.md                               This file
├── AGENTS.md                               Consolidated drop-in template
├── patterns/
│   ├── 01-trust-boundary.md                User intent vs. third-party content
│   ├── 02-confirmation-hygiene.md          Just-in-time confirmation
│   ├── 03-sensitive-data-transmission.md   Sensitive data and consent
│   └── 04-prompt-injection-handling.md     Injection detection and escalation
├── policies/
│   ├── action-risk-tiers.md                Hand-off, always-confirm, pre-approved
│   ├── environment-isolation.md            Browser or VM hardening and allow lists
│   ├── turn-discipline.md                  Final-answer and termination rules
│   └── visual-input-hygiene.md             Screenshot, OCR, and detail settings
├── checklists/
│   └── implementation-checklist.md         Pre-ship verification
├── CONTRIBUTING.md                         How to contribute new patterns
└── LICENSE                                 MIT
```

## The four core patterns at a glance

| # | Pattern | Protects against | Fires when |
|---|---------|------------------|------------|
| 1 | [Trust boundary](./patterns/01-trust-boundary.md) | Prompt injection, social engineering from pages, docs, emails | Agent reads any non-user content |
| 2 | [Confirmation hygiene](./patterns/02-confirmation-hygiene.md) | Premature asks, bundled risky actions, typing sensitive data without consent | Immediately before a risky action |
| 3 | [Sensitive data and transmission](./patterns/03-sensitive-data-transmission.md) | Data exfiltration, accidental sharing, fabricated PII | Before any form fill, URL visit, upload, or share |
| 4 | [Prompt injection handling](./patterns/04-prompt-injection-handling.md) | Instruction overrides hidden in pages, UI, or tool outputs | On-screen text tries to steer the agent off-task |

## How to use the blocks

**Option A: One file, drop-in.** Copy [`AGENTS.md`](./AGENTS.md) into your repo root. Most agent frameworks (Claude Code, OpenAI Codex, Cursor, Cline) will pick it up automatically as agent-level instructions.

**Option B: Modular.** Pick the individual pattern and policy blocks you need and compose your own system prompt. Use this when you already have an agent policy and just want to layer in specific protections.

**Option C: Reference spec.** Keep the repo as a reference your security reviewers point to during agent design reviews. Link to specific blocks in pull-request comments.

## Source and attribution

The four core patterns are adapted from OpenAI's Computer Use guide, which publishes them explicitly as prompt excerpts meant to be incorporated into agent instructions:

https://developers.openai.com/api/docs/guides/tools-computer-use

Supporting policy blocks (action risk tiers, environment isolation, turn discipline, visual input hygiene) are derived from the same guide's "Handle user confirmation and consent" and "Keep a human in the loop" sections. Wording has been reorganized and extended for modularity.

Before you ship an agent, review:
- OpenAI Usage Policy: https://openai.com/policies/usage-policies/
- OpenAI Business Terms: https://openai.com/policies/business-terms/
- Your own organization's AI and data-handling policies

## Contributing

Pull requests are welcome. See [`CONTRIBUTING.md`](./CONTRIBUTING.md) for the bar for new patterns. The short version: every pattern should be drop-in, vendor-neutral where possible, and tied to a concrete threat.

## License

CC BY 4.0. See [`LICENSE`](./LICENSE). You can use these blocks in commercial and non-commercial projects without attribution, though attribution is appreciated.

## License

This work is licensed under the [Creative Commons Attribution 4.0 International License (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/). See [`LICENSE`](./LICENSE) for the full text.

You are free to use, share, and adapt these patterns for any purpose, including commercial use, provided you give appropriate credit.

**Suggested attribution:**

> Adapted from [agent-safety-prompts](https://github.com/anshug/agent-safety-prompts/) by Anshu Gupta, licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).

For inline use in a system prompt or `AGENTS.md` file, a single-line credit is enough:

```
# Adapted from aisecuritydevprompts (https://github.com/anshug/aisecuritydevprompts) by Anshu Gupta, CC BY 4.0
```

## Maintainer

Curated by [Anshu Gupta](https://github.com/anshug), Founder and CISO, Fixin Security. Part of the broader [Tejas Cyber Network](https://www.tejascybernetwork.com) community resource set.
