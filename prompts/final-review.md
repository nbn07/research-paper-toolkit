You are running a fast, single-pass, deterministic final checklist on my IEEE or ACM paper, right before I submit it. This is meant to complement a deeper independent review (see the fresh-eyes-paper-review prompt in this pack), not replace it — if I haven't run that yet on a paper that matters, tell me so. Think of this as the last-mile sanity check, not a simulated multi-reviewer review.

## What I'll give you

- The paper (as much of the full text as I can paste — figure/table captions help even if you can't see the images themselves).
- Optionally, what I believe the paper's core contribution/novelty is, so you can compare that claim against what the text actually delivers.

## The eight checks

For each one, look at the actual relevant part of the paper I pasted — don't just summarize impressions — and give a verdict with evidence (quote the specific sentence, section, or describe the specific figure/table). Never state a problem without pointing to exactly where it is.

1. **Contribution to the body of knowledge.** Does the paper state a clear, specific technical contribution — not just "we propose a system for X"? Compare what the introduction *claims* against what the method and results sections actually *deliver*. A mismatch between the two is a real finding. Don't penalize an honestly-labeled incremental improvement just for being incremental — flag it only if it's presented as more than it is.
2. **Technical soundness.** Are the equations, algorithms, and experimental methods internally consistent — every symbol defined before use, no contradiction between how a mechanism is described in one place versus another? Flag unrealistic baseline assumptions or a dataset description missing key details (size, source, labeling method).
3. **Baseline comparisons and validation.** Is the method compared against the strongest *relevant* recent prior work, not just an easy or outdated baseline? Flag a thin evaluation — too few runs, no variance reported, no ablation where one would isolate the actual contribution.
4. **Relevance and sufficiency of citations.** Does the paper engage with recent work (last 2-3 years), not only older foundational work, and cover the field's recognized milestones? Flag a related-work section that's just a list of citations without sentences explaining how this paper differs from each.
5. **Citation ethics.** Look for citation stuffing — citations that don't actually support the sentence they're attached to, or a suspicious concentration of citations toward one source/author beyond what the content justifies. (This is different from checking whether the citations are real — that's the citation-verifier prompt's job. This is about *why* a citation is there.)
6. **Structure and readability.** Does the paper follow a clear flow (Introduction → Related Work → Method → Results → Discussion → Conclusion, or the venue's expected variant) with signposting between sections, or does a reader have to work to follow the argument?
7. **Visual presentation.** Are figures and tables legible and necessary, with captions in the position the venue's style expects, and is every figure/table actually referenced from the body text? Flag anything purely decorative.
8. **Language quality.** Is the English clear enough that the technical content isn't obscured by grammar or phrasing? This is about clarity, not style preference — flag only passages where a reader could genuinely misread the technical claim, not minor awkwardness.

## How to report back

```
# Final Review

## Summary
<N passed, M warnings, K blocking>

## Blocking issues
| Check | Finding | Evidence (quote/location) |

## Warnings
| Check | Finding | Evidence (quote/location) |

## Passed checks
<compact list, one line each>
```

Then a short verbal summary: ready to submit or not, and the single most important thing to fix first.

## Rules

- This is a checklist pass, not a plagiarism detector, an AI-content classifier, or a fact-checking service — you have no way to verify a claim against ground truth you weren't given. If I ask whether the paper is "100% original" or "fully verified," tell me plainly this check can't produce that kind of certainty, and explain what it actually covers.
- Don't rewrite the paper during this pass; list findings and let me decide what to change.
- If I haven't run the citation-verifier or venue-compliance checks yet, say so — reference accuracy and formatting/length compliance are out of scope here.
