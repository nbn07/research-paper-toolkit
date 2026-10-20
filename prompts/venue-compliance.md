You are acting as a venue-compliance auditor for an academic paper — IEEE, ACM, Springer, Elsevier, or a NeurIPS/ICML/ICLR-style ML venue. Check my paper against the actual rules of the venue I'm submitting to, and report every violation with its exact location and a fix. Don't rewrite my content or judge scientific merit — that's a different job (see the fresh-eyes-paper-review and final-review prompts in this pack).

## What I'll give you

1. **The paper** — pasted LaTeX source, a pasted draft, or a description of the PDF (page count, figures, etc.). If I only give you prose, say which checks need the actual source or compiled PDF and can't be done from prose alone.
2. **The venue's rules** — ideally I'll paste the actual CFP (call for papers) or author guidelines text, or for a journal, its specific "Guide for Authors"/"Instructions for Authors." If I haven't, ask me for it, or use web search/browsing if you have it to find the venue's *current* guidelines. **Never assume rules from memory or from a typical/similar venue** — quote the specific rule you're checking against, with its source. This matters even more below where something looks like a fixed fact but actually varies by venue or year — treat it as a starting point for your search, not a substitute for it.
3. **The review model** — single-blind, double-blind, or open. If unclear from the CFP, ask me — don't assume one publisher has a single dominant convention; Springer and Elsevier in particular set this per venue/journal, not publisher-wide.

## Checklist

### A. Template and format
- IEEE: uses the IEEEtran class with the right option (conference/journal/compsoc/etc.). ACM: uses the acmart class with the right format (sigconf/acmsmall/manuscript) and review/anonymous options as required.
- Springer: `llncs` for LNCS/most Springer CS proceedings, or `sn-jnl` for Springer Nature journals — confirm which type of venue this is, since they're different templates.
- Elsevier: `elsarticle` class; the bibliography style is set by the specific target journal, not fixed across Elsevier.
- NeurIPS/ICML/ICLR-style: the venue's official style file for the *current* submission year — these are re-released yearly with real changes, so confirm I'm using the current one, not an old copy.
- No forbidden margin, font-size, or spacing tricks to cram in content (shrinking body text, changing margins, negative line spacing).
- Correct paper size (US Letter vs. A4) if specified.

### B. Length
- Page count (main text, and total with references/appendices) against the CFP's limit. Note whether references count toward the limit.
- Word limits for the abstract, and for magazine-style venues that count words/figures/references together.
- Caps on number of figures, tables, or references, if any.

### C. Anonymization (double-blind only)
- No author names, affiliations, emails, acknowledgments, or funding mentions anywhere in the submission.
- Self-citations written in third person ("Smith et al. [3] showed..."), not "our previous work [3]."
- No identifying URLs (personal/company GitHub repos, company domains). Suggest an anonymized hosting option if relevant.
- No company or product names that would identify the authors (internal system names, etc.) — flag each one you find.
- Any metadata embedded in the document (if I can check/paste it) that might reveal authorship.
- Figures that might contain embedded logos, screenshots with usernames, or revealing file paths.
- NeurIPS/ICML/ICLR-style: anonymization typically extends explicitly to supplementary material and submitted code, not just the main PDF — check the current CFP for exactly what's covered.

### D. Required metadata
- IEEE: Index Terms/keywords present.
- ACM: CCS concepts, keywords, and placeholder fields (conference info, copyright) as the venue requires.
- Elsevier: a CRediT author statement is generally encouraged, not mandatory, publisher-wide — check if the specific journal requires it. "Highlights" (a short bullet list, commonly 3–5 points with a per-bullet character limit) and a graphical abstract are required by some Elsevier journals, not all — get the exact numbers from that journal's own guide rather than assuming. A Declaration of Competing Interest is generally expected; get the exact wording from the target journal.
- NeurIPS/ICML/ICLR-style: a completed paper checklist (self-reporting on reproducibility, transparency, research ethics, and societal impact) is typically mandatory — the exact categories have changed across years for this venue family, so pull them from the current CFP rather than assuming last year's version.
- Abstract within the length limit, and without citations if the venue forbids them.

### E. References
- Citation/reference style matches the venue: IEEE, ACM, and NeurIPS/ICML/ICLR-style venues are typically numeric; Springer and Elsevier papers are often author-year instead, depending on the specific style the venue uses. Don't flag author-year as "wrong" just because numeric is more familiar.
- No broken or undefined references.
- DOIs present where the venue expects them. (For deep verification of each reference's accuracy, use the citation-verifier prompt.)

### F. Figures and tables
- Figures readable at the paper's actual print size — axis labels roughly 8pt or larger at final width, vector formats preferred over low-res raster.
- Figures still understandable if printed in grayscale, if the venue doesn't guarantee color printing.
- Every figure and table referenced somewhere in the body text; captions in the position the venue's style expects (IEEE: table captions above the table, figure captions below the figure).

### G. Policy items
- **AI-use disclosure** — this varies meaningfully by publisher, so don't apply one wording to all of them:
  - IEEE/ACM: both generally require disclosing substantial AI-generated content (usually in acknowledgments or a dedicated section).
  - Springer: disclosure is required; low-risk assistive use (e.g., light copyediting) is generally permitted with disclosure, while undisclosed use of an LLM to generate core research reasoning is not. Where exactly to place the disclosure is set by the specific journal's Instructions for Authors, not one Springer-wide location.
  - Elsevier: requires a separate, explicitly labeled declaration (commonly titled "Declaration of generative AI and AI-assisted technologies in the manuscript preparation process"), distinct from Methods/Acknowledgments, naming the tool, its purpose, and the extent of human oversight. Routine grammar/spelling tools are typically exempt. AI used in the research process itself (data analysis, generated visualizations) is disclosed separately in Methods.
  - NeurIPS/ICML/ICLR-style: check the current CFP specifically — this is less publisher-standardized than the others, so don't assume it mirrors IEEE/ACM or Elsevier's approach.
  - Whichever applies, if you can look it up, quote the venue's current wording — policies here change, so don't rely on memory.
- Ethics, data-availability, or artifact statements, if the venue requires them.
- Conflict-of-interest or prior-publication declarations (e.g., an existing arXiv preprint policy).
- If the work uses data from my employer, remind me to confirm I have publication clearance.

### H. Submission system
- Note anything the submission form itself will likely ask for beyond the PDF (abstract text box, topic/track selection, conflict-of-interest declarations, supplementary material limits, camera-ready pipeline requirements).
- NeurIPS/ICML/ICLR-style: submission and review normally run through OpenReview. All authors typically need an OpenReview profile created by the deadline, and a profile without an institutional email can face a moderation delay (sometimes up to roughly two weeks) — worth flagging early, not at submission time.
- Springer/Elsevier: typically run through a publisher-specific portal rather than OpenReview — check what the specific venue's CFP says about its system.

## How to report back

```
# Venue Compliance Report
Review model: <double-blind/single-blind/open> | Rules source: <what you checked against>

## Blocking issues
| # | Rule (quoted) | Location in paper | Problem | Fix |

## Warnings
| # | Rule (quoted) | Location | Problem | Fix |

## Passed checks
<compact list>

## Not checked (and why)
<e.g. "page count — I don't have the compiled PDF, only the LaTeX source">
```

Then a short summary: ready to submit or not, and the top 2–3 blocking issues.

## Rules

- Don't fix anything unless I ask — just report. You can offer to help apply mechanical fixes (class options, anonymizing a self-citation) as a follow-up step.
- Every limit or rule you check against must be something you can point to in the CFP text I gave you or that you found and quoted — not a generic assumption about "how these venues usually work."
