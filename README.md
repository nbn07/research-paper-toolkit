# Research Paper Toolkit (IEEE / ACM / Springer / Elsevier / NeurIPS-style)
If you found this project useful, please consider giving it a ⭐ to help others find it!

A Claude Code / Claude Cowork plugin that takes a research paper from venue selection through drafting, citation verification, formatting compliance, independent review, and submission — as five skills that hand off to each other at the right stage.

Built around one rule: **never invent a citation, a result, or a fact about the author's own system.** Every skill here is designed to say "unverified" or "ask the author" rather than fill a gap with something plausible-sounding.

It covers IEEE, ACM, Springer, Elsevier, and NeurIPS/ICML/ICLR-style ML venues: venue-specific checks (IEEEtran vs. acmart vs. llncs/sn-jnl vs. elsarticle vs. this year's conference style file; numeric vs. author-year citation styles; IEEE Index Terms vs. ACM CCS concepts vs. Elsevier's CRediT/Highlights vs. the NeurIPS-style paper checklist; IEEE PDF eXpress vs. ACM TAPS camera-ready checks) are built into the workflow and compliance skills, and the review skills are venue-agnostic. Because publisher policies and yearly style files change, `venue-compliance` is built to always quote the current CFP or author guide rather than assume a fixed rule — see that skill for specifics on what's stable background knowledge versus what it always re-checks.

**Not on Claude?** See [Using this without Claude](#using-this-without-claude) below — a plain-prompt version of the same methodology that works with any chatbot.

## What's included

| Skill | What it does |
|---|---|
| `research-paper-workflow` | The orchestrator. Figures out what stage your paper is at (venue selection, lit review, drafting, review, submission, rebuttal) and routes to the right skill below, or does that stage's work directly. |
| `citation-verifier` | Checks every entry in your `.bib` file against a real, retrieved source — flags fabricated, mismatched, retracted, or unsupported citations. Never confirms a reference from memory. |
| `venue-compliance` | Audits your paper against the *actual* target venue's CFP — page limits, template, anonymization for double-blind, required metadata, reference style, AI-disclosure policy — and quotes the rule it's checking against. Covers IEEE, ACM, Springer, Elsevier, and NeurIPS/ICML/ICLR-style conventions. |
| `fresh-eyes-paper-review` | Runs several independent reviewer personas (skeptical PC member, domain practitioner, methods expert, confused newcomer) who see *only* the final paper, never the drafting conversation, then consolidates their findings into a prioritized fix list. |
| `final-review` | A fast, single-pass checklist for the last mile: contribution clarity, technical soundness, baseline sufficiency, citation ethics, structure, visuals, language. Complements fresh-eyes rather than duplicating it. |

## What this toolkit is honest about not doing

- It is **not** a plagiarism detector, an AI-content classifier, or a fact-checking oracle. No such tool is wired in. "Verified" means a citation was traced to a real, retrieved source — not that the paper is guaranteed original or that every claim is independently fact-checked.
- Simulated reviewers (`fresh-eyes-paper-review`) reduce author blindness. They are not a substitute for a human domain expert reading your paper before you submit it.
- Every skill is told to ask or flag rather than guess when something is missing — expect to be asked for the CFP, your data, or a decision on an unresolved point rather than having it quietly filled in.

## Requirements

- Claude Code or Claude Cowork with plugin support.
- Nothing else is strictly required — all five skills fall back to general web search when a specialized tool isn't connected.
- Optional, and used automatically if connected: **Consensus**, **alphaXiv**, and **SciSpace** MCP connectors speed up literature search and citation verification. A local LaTeX toolchain (`latexmk`, `pdffonts`) lets `venue-compliance` check compiled page count and font embedding directly; without it, the skill says which checks it skipped.

## Install

```bash
# Register this repo as a marketplace (one time)
claude plugin marketplace add nbn07/research-paper-toolkit

# Install the plugin from it
claude plugin install research-paper-toolkit@research-paper-toolkit
```

Or, inside a Claude Code session (v2.1.275+):

```
/plugin install research-paper-toolkit --marketplace nbn07/research-paper-toolkit
```

To try it without installing anything permanently, clone the repo and load it for one session:

```bash
claude --plugin-dir ./research-paper-toolkit
```

> Already have the previous version (`ieee-research-paper-toolkit`) installed? This repo's marketplace maps that old name to the new one, so updating your marketplace will migrate your install automatically — you don't need to reinstall from scratch.

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
research-paper-toolkit/
├── .claude-plugin/
│   ├── plugin.json         # plugin manifest
│   └── marketplace.json    # lets this same repo be installed by name
├── skills/                 # Claude Code / Cowork skills (auto-discovered)
│   ├── research-paper-workflow/SKILL.md
│   ├── citation-verifier/SKILL.md
│   ├── venue-compliance/SKILL.md
│   ├── fresh-eyes-paper-review/SKILL.md
│   └── final-review/SKILL.md
├── prompts/                 # plain-text version for any other chatbot
│   ├── README.md
│   ├── research-paper-workflow.md
│   ├── citation-verifier.md
│   ├── venue-compliance.md
│   ├── fresh-eyes-paper-review.md
│   └── final-review.md
├── LICENSE
└── README.md
```

## Using this without Claude

Everything above is a Claude Code / Cowork plugin — it only auto-activates inside Claude, where `SKILL.md` files are discovered and routed automatically. If you use ChatGPT, Gemini, Copilot Chat, a local model, or anything else, the **`prompts/`** folder has the same five pieces of methodology rewritten as plain instructions, with no Claude-specific tool calls, file I/O, or automatic skill hand-offs assumed.

To use them: open `prompts/README.md` for the full explanation, then paste the relevant file's contents as your first message (or into your assistant's system-prompt/custom-instructions field) in a new chat, followed by your paper, bibliography, or CFP text as appropriate.

This covers the same ground — the IEEE/ACM domain checks, the verdict categories, the "never invent a citation or result" rule — adapted for assistants that may not have web access, file access, or the ability to run several independent reviewer passes in parallel. Where a capability isn't available, the prompts are written to say so rather than fake it.

## Contributing

Issues and PRs welcome — especially real paper drafts (anonymized or synthetic) that expose gaps in the checks, additional venue-specific guidance (magazine-style IEEE venues, ACM SIG-specific templates, individual Springer/Elsevier journal quirks, or the ICML/ICLR variants of the NeurIPS-style paper checklist), or improvements to the `prompts/` versions for specific other assistants.

## License

MIT — see [LICENSE](LICENSE).

---

If this saved you a round of citation-fixing or a desk-reject, a star helps others find it.
