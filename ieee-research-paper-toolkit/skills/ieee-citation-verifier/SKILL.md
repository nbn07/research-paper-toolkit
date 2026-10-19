---
name: "ieee-citation-verifier"
description: "Verify every reference in a paper's .bib file or bibliography against real sources and flag fabricated, mismatched, or incomplete citations. Trigger on requests like 'check my references', 'verify this bibliography', 'did I cite this right', or before any submission — a fabricated or mismatched citation is a desk-reject and integrity risk, not just a formatting nit."
---

# Citation Verifier

Goal: every reference in the paper exists, its metadata is correct, and it supports the claim it is cited for. Hallucinated or mangled references are a desk-reject and integrity risk, so be strict: if a reference can't be confirmed from a real source, it is **Unverified**, never assumed correct.

## Inputs

- `refs.bib` (or the bibliography section of the paper or PDF).
- `main.tex` and section files, for checking that each citation fits the sentence it supports.
- Optional scope from the user: "only new entries", "only Section II", and so on.

## Tools to use

Treat general web search/fetch and DOI resolution as the baseline that always works, regardless of what's connected in a given session:

1. **Web search / fetch** for DOI resolution (doi.org, Crossref), IEEE Xplore, the ACM Digital Library, DBLP, arXiv, and standards bodies (RFCs, 3GPP, ETSI, RDK documentation). This alone is enough to verify most entries.
2. **Consensus** (`search`), if connected, for faster peer-reviewed lookup.
3. **alphaXiv**, if connected, for arXiv preprints and full text.

Never confirm a reference from memory alone, and never skip verification just because a specialized tool isn't connected — fall back to web search/fetch instead. Each verdict needs a source URL you actually retrieved.

## Per-entry checks

For each BibTeX key:

1. **Existence:** find the work by title (and DOI if one is given).
2. **Metadata match:** check title (allowing for capitalization), author list and order, year, venue or journal name, volume, issue and pages, and DOI. Note each mismatch.
3. **Preprint vs published:** if an arXiv entry has since been published at a venue, recommend citing the published version (and give its metadata).
4. **Retraction or errata:** flag any notice you find.
5. **Entry quality:** right entry type (`@inproceedings` vs `@article` vs `@misc`), required fields present, title capitals protected with `{...}` for acronyms (for example `{IEEE}`, `{5G}`, `{RDK}`), consistent venue naming, no duplicate entries under different keys.
6. **Usage:** cited in the text at least once (flag unused entries) and defined (flag `\cite` keys missing from the `.bib`).
7. **Claim support (sampled or full, as the user prefers):** read the sentence that cites it and check the abstract or full text. Mark **Supports / Partially / Does not support / Couldn't check**. Look hardest at numeric claims ("reduces latency by 40% [12]").
8. **Recency and balance (paper-level):** note if the related work is missing important recent work (past 2–3 years) from the target venue. Suggest at most 5 candidates, each verified.

## Verdicts

- ✅ **Verified:** exists, metadata correct.
- 🟡 **Verified with fixes:** exists, but the metadata needs corrections (list them).
- 🔴 **Unverified:** couldn't be found in any source. Treat as possibly fabricated, and have the author confirm it or remove it.
- ⚠️ **Problem:** retracted, or doesn't support the claim, or is a preprint where a published version exists.

## Output

1. Write `citation-report.md` in the paper folder:

```
# Citation Verification — <paper>
Checked: <date> | Entries: <N> | ✅ <a> 🟡 <b> 🔴 <c> ⚠️ <d>

## Action required (🔴 and ⚠️)
| Key | Issue | Evidence / searched sources | Recommended action |

## Metadata fixes (🟡)
| Key | Field | Current | Correct | Source URL |

## Usage issues
- Unused entries: ...
- Undefined \cite keys: ...
- Duplicates: ...

## Claim-support findings
| Key | Cited sentence (location) | Verdict | Note |

## Verified (✅)
<key — source URL>
```

2. Offer to write a corrected `refs.verified.bib` with the 🟡 fixes applied. Never overwrite `refs.bib` unless the user asks, and never silently delete 🔴 entries.
3. Summarize in chat in 2–3 sentences: counts, and the entries needing the author's attention.

## Rules

- Don't "repair" a 🔴 entry by swapping in a similar real paper without the user's approval. The author may have meant a different work.
- Quote exact titles and DOIs from the retrieved source. Don't paraphrase metadata.
- For large bibliographies (more than 40 entries), process them in batches and give progress updates.
- This checks *existence, accuracy, and claim support* — it is not a plagiarism detector and doesn't guarantee the author's prose around a citation is original. Say so if asked to do more than this skill covers.
