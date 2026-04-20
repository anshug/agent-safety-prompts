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
- Content copied from third-party sources without a license that permits redistribution under CC BY 4.0

## Pull request process

1. Open an issue first if the change is substantial. Describe the threat, the pattern, and the harness assumptions.
2. Keep PRs scoped. One pattern or one policy block per PR.
3. Match the existing file structure: each pattern or policy file has a category, purpose, drop-in block, adaptation notes, and related links section.
4. Do not use em dashes in content. Use regular dashes or rewrite the sentence.
5. Cite your sources. If the pattern is adapted from a public guide, link it. Confirm in the PR description that any adapted content is licensed compatibly with CC BY 4.0.

## Style

- Tone: direct, declarative, practical. This is reference material, not a blog post.
- Headings: sentence case.
- Code blocks: use fenced markdown blocks with no syntax highlight for policy text, so it copies cleanly into system prompts.
- Length: a pattern file should be readable in under five minutes.

## Licensing

This repository is licensed under the Creative Commons Attribution 4.0 International License (CC BY 4.0). See [`LICENSE`](./LICENSE) for the full text and https://creativecommons.org/licenses/by/4.0/ for a plain-language summary.

By submitting a contribution, you agree that:

1. Your contribution will be licensed under CC BY 4.0, the same license as the rest of the repository.
2. You have the right to submit the contribution. It is either your original work, or you are authorized by the copyright holder to submit it under CC BY 4.0.
3. You retain copyright to your contribution. The license grants downstream users permission to use, share, and adapt your work, provided they give appropriate credit.

If your contribution includes adapted content from another source, you must:

- Name the source in the PR description, with a link
- Confirm that the source license is compatible with CC BY 4.0 (for example: public domain, CC0, CC BY 4.0, or content you authored and own)
- Preserve any attribution required by the source license within the contribution itself

Contributions that bundle content under incompatible terms (for example, CC BY-NC, CC BY-ND, GPL-style share-alike for non-code content, proprietary, or unlicensed third-party text) cannot be accepted.

## Attribution for contributors

Contributors are credited in the following ways:

- The git commit history preserves your authorship on every merged PR
- Significant contributors will be listed in a `CONTRIBUTORS` file (to be added when contributions begin)
- For patterns primarily authored by a contributor, the pattern file may include an `Author:` line in the header

If you prefer a different credit format, for example a pseudonym or withholding your name entirely, note the preference in your PR description and we will honor it.

## Questions

Open a discussion on the repo or reach out through the Tejas Cyber Network community channels.
