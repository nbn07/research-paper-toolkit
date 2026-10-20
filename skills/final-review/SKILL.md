---
name: "final-review"
description: "Run a fast, deterministic final checklist pass on an IEEE/ACM paper — contribution clarity, technical soundness, baseline sufficiency, citation ethics, structure, visual presentation, and language quality. Trigger on requests like 'final check before I submit', 'one more pass over this paper', or 'is anything obviously missing' — use this after fresh-eyes-paper-review as a quick last-mile audit, not a replacement for it; it's a single-pass checklist, not a simulated multi-reviewer review."
---

# Final Review Checklist

Purpose: a fast, single-pass audit against eight concrete criteria, run last, right before submission. This complements **fresh-eyes-paper-review** rather than duplicating it — fresh-eyes simulates independent reviewers reading the paper cold; this skill runs a direct, deterministic checklist yourself, in one pass, as a final sanity check that nothing obvious was missed. Run fresh-eyes first on any paper that matters; use this as the quick last-mile pass, or on its own for a fast check mid-draft.

## Inputs

- The paper (PDF preferred, since figure/table layout and page count are only reliable there; LaTeX source if a PDF isn't available).
- The venue's claimed novelty/contribution, if the user has stated it, so "Check 1" can compare the paper's claim against what the text actually delivers rather than against your own guess at what would be impressive.

## The eight checks

For each one, read the relevant part of the paper directly — don't rely on a summary — and record a verdict with evidence (section, page, or a short quote). Never assert a problem without pointing to the specific sentence or figure that shows it.

1. **Contribution to the body of knowledge.** Does the paper state a clear, specific technical contribution (not just "we propose a system for X")? Compare what the introduction *claims* against what the method and results sections actually *deliver* — a mismatch between the two is a real finding, not a style note. Flag incremental contributions presented as breakthroughs, but don't penalize an honestly-labeled incremental improvement for being incremental.
2. **Technical soundness.** Are equations, algorithms, and experimental methods internally consistent — every symbol defined before use, no equation in one section contradicted by a different description of the same mechanism elsewhere (for example, a method described as a fixed formula in one place and a learned model in another)? Flag unrealistic baseline assumptions or an incomplete dataset description (missing size, source, or labeling method).
3. **Baseline comparisons and validation.** Is the proposed method compared against the strongest *relevant* recent prior work, not just an easy or outdated baseline? Flag a thin evaluation (too few runs, no variance reported, no ablation when one would isolate the actual contribution).
4. **Relevance and sufficiency of citations.** Are citations current (the paper engages with work from the last 2–3 years, not only older foundational work) and does it cover the field's recognized milestones? Flag a related-work section that reads as a citation list without positioning sentences explaining how this paper differs from each.
5. **Citation ethics.** Look for citation stuffing — citations added that don't support the sentence they're attached to, or a suspicious concentration of citations to one source/author beyond what the content justifies. This is a different, narrower check than citation-verifier's existence/accuracy verification: this one is about *why* a citation is there, not whether it's real.
6. **Structure and readability.** Does the paper follow a clear, standard flow (Introduction → Related Work → Method → Results → Discussion → Conclusion, or the venue's expected variant) with signposting between sections, or does a reader have to work to follow the argument?
7. **Visual presentation.** Are figures and tables legible and necessary? Captions in the right place (IEEE: table captions above the table, figure captions below the figure) and every figure/table referenced from the body text. Flag anything decorative that doesn't carry information.
8. **Language quality.** Is the English clear enough that technical content isn't obscured by grammar or phrasing? This is about clarity, not style preference — flag only passages where a reader could genuinely misunderstand the technical claim, not minor awkwardness.

## Output

Report in chat (no file needed for this quick pass, unless the user asks for one):

```
# Final Review — <paper>

## Pass / Warning / Blocking summary
<N passed, M warnings, K blocking>

## Blocking issues
| Check | Finding | Evidence (section/quote) |

## Warnings
| Check | Finding | Evidence (section/quote) |

## Passed checks
<compact list, one line each>
```

Then a 2–3 sentence verbal summary: ready to submit or not, and the single most important thing to fix first.

## Rules

- This is a checklist pass, not a plagiarism detector, an AI-content classifier, or a fact-checking service — it has no way to verify a claim against ground truth it wasn't given. If asked whether the paper is "100% original" or "accuracy-verified," say plainly that this check doesn't produce that kind of certainty and explain what it actually covers.
- Don't rewrite the paper during this pass; list findings and let the user decide what to change.
- If `citation-verifier` and `venue-compliance` haven't been run yet, say so — citation existence/accuracy and formatting/length compliance are out of scope here and belong to those skills.
