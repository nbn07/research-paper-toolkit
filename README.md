# IEEE/ACM Research Paper Toolkit

A Claude Code / Claude Cowork plugin that takes an IEEE or ACM research paper from venue selection through drafting, citation verification, formatting compliance, independent review, and submission — as five skills that hand off to each other at the right stage.

Built around one rule: **never invent a citation, a result, or a fact about the author's own system.** Every skill here is designed to say "unverified" or "ask the author" rather than fill a gap with something plausible-sounding.

## What's included

| Skill | What it does |
|---|---|
| `ieee-research-paper-workflow` | The orchestrator. Figures out what stage your paper is at (venue selection, lit review, drafting, review, submission, rebuttal) and routes to the right skill below, or does that stage's work directly. |
| `ieee-citation-verifier` | Checks every entry in your `.bib` file against a real, retrieved source — flags fabricated, mismatched, retracted, or unsupported citations. Never confirms a reference from memory. |
| `ieee-venue-compliance` | Audits your paper against the *actual* target venue's CFP — page limits, template, anonymization for double-blind, required metadata, reference style, AI-disclosure policy — and quotes the rule it's checking against. |
| `ieee-fresh-eyes-paper-review` | Runs several independent reviewer personas (skeptical PC member, domain practitioner, methods expert, confused newcomer) who see *only* the final paper, never the drafting conversation, then consolidates their findings into a prioritized fix list. |
| `ieee-final-review` | A fast, single-pass checklist for the last mile: contribution clarity, technical soundness, baseline sufficiency, citation ethics, structure, visuals, language. Complements fresh-eyes rather than duplicating it. |

## What this toolkit is honest about not doing

- It is **not** a plagiarism detector, an AI-content classifier, or a fact-checking oracle. No such tool is wired in. "Verified" means a citation was traced to a real, retrieved source — not that the paper is guaranteed original or that every claim is independently fact-checked.
- Simulated reviewers (`ieee-fresh-eyes-paper-review`) reduce author blindness. They are not a substitute for a human domain expert reading your paper before you submit it.
- Every skill is told to ask or flag rather than guess when something is missing — expect to be asked for the CFP, your data, or a decision on an unresolved point rather than having it quietly filled in.

## Requirements

- Claude Code or Claude Cowork with plugin support.
- Nothing else is strictly required — all five skills fall back to general web search when a specialized tool isn't connected.
- Optional, and used automatically if connected: **Consensus**, **alphaXiv**, and **SciSpace** MCP connectors speed up literature search and citation verification. A local LaTeX toolchain (`latexmk`, `pdffonts`) lets `ieee-venue-compliance` check compiled page count and font embedding directly; without it, the skill says which checks it skipped.

## Install

```bash
# Register this repo as a marketplace (one time)
claude plugin marketplace add nbn07/ieee-research-paper-toolkit

# Install the plugin from it
claude plugin install ieee-research-paper-toolkit@ieee-research-paper-toolkit
```

Or, inside a Claude Code session (v2.1.275+):

```
/plugin install ieee-research-paper-toolkit --marketplace nbn07/ieee-research-paper-toolkit
```

To try it without installing anything permanently, clone the repo and load it for one session:

```bash
claude --plugin-dir ./ieee-research-paper-toolkit
```

## Quick start

Some prompts that route to the right skill automatically:

- *"I'm starting a new paper on \<topic\> for IEEE/ACM — help me pick a venue."*
- *"Here's Section III of my draft. Tighten it to 900 words without losing claims."*
- *"Check my references in refs.bib."*
- *"Is this ready to submit to \<venue\>? Here's the CFP."*
- *"Give this an independent review before I submit — simulate reviewers who haven't seen our conversation."*
- *"One more pass — anything obviously missing before I submit?"*

## Repo layout

```
ieee-research-paper-toolkit/
├── .claude-plugin/
│   ├── plugin.json        # plugin manifest
│   └── marketplace.json   # lets this same repo be installed by name
├── skills/
│   ├── ieee-research-paper-workflow/SKILL.md
│   ├── ieee-citation-verifier/SKILL.md
│   ├── ieee-venue-compliance/SKILL.md
│   ├── ieee-fresh-eyes-paper-review/SKILL.md
│   └── ieee-final-review/SKILL.md
├── LICENSE
└── README.md
```

## Contributing

Issues and PRs welcome — especially real paper drafts (anonymized or synthetic) that expose gaps in the checks, or additional venue-specific guidance (magazine-style IEEE venues, ACM SIG-specific templates, etc.).

## License

MIT — see [LICENSE](LICENSE).

---

If this saved you a round of citation-fixing or a desk-reject, a star helps others find it.
