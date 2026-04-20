# Contributing

Thanks for considering a contribution. This repo aims to be a curated, high-signal reference for agent safety prompt patterns. The bar for a new pattern is deliberately high so the set stays usable.

## What we accept

**New prompt patterns.** A new pattern should:

- Address a concrete, recurring threat that existing patterns do not cover
- Be drop-in: the pattern block can be pasted into a system prompt without modification
- Be vendor-neutral where possible, or clearly note which harness or model it assumes
- Include a "why it matters" section that ties the pattern to a specific threat
- Include adaptation notes so other builders can tune it

**Extensions to existing patterns.** Adaptation notes, product-specific examples, and red-team findings are welcome. Open a PR against the relevant pattern file.

**Policy blocks.** Supporting blocks that sit alongside the core patterns (environment, turn discipline, data handling, and so on). Same quality bar as patterns.

**Checklist items.** New items for the implementation checklist must tie back to a pattern or policy block in the repo. No orphan checks.

## What we do not accept

- Model-vendor marketing material
- Sweeping policy statements that are not concrete enough to encode in a prompt
- Patterns that restate existing patterns with different wording
- Patterns that depend on closed-source or proprietary tooling to verify

## Pull request process

1. Open an issue first if the change is substantial. Describe the threat, the pattern, and the harness assumptions.
2. Keep PRs scoped. One pattern or one policy block per PR.
3. Match the existing file structure: each pattern or policy file has a category, purpose, drop-in block, adaptation notes, and related links section.
4. Do not use em dashes in content. Use regular dashes or rewrite the sentence.
5. Cite your sources. If the pattern is adapted from a public guide, link it.

## Style

- Tone: direct, declarative, practical. This is reference material, not a blog post.
- Headings: sentence case.
- Code blocks: use fenced markdown blocks with no syntax highlight for policy text, so it copies cleanly into system prompts.
- Length: a pattern file should be readable in under five minutes.

## Licensing

By contributing you agree that your contribution is licensed under the same MIT License as the rest of the repo.

## Questions

Open a discussion on the repo or reach out through the Tejas Cyber Network community channels.
