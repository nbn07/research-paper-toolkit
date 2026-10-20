---
name: "venue-compliance"
description: "Audit an IEEE or ACM paper (LaTeX or PDF) against the target venue's call for papers and author guidelines before submission. Trigger on requests like 'is this ready to submit', 'check the page limit', 'did I anonymize this correctly', or 'does this meet the CFP requirements' — checks formatting, length, anonymization, required metadata, references, figures, and policy items against the venue's actual rules, not generic defaults."
---

# Venue Compliance Audit

Check a paper against the target venue's submission rules and report every violation with its exact location and a fix. Do not rewrite content or judge scientific merit; that is the review skills' job.

## Inputs

1. **The paper:** `main.tex` (plus any `\input` files, `refs.bib` and `figures/`) or the compiled PDF. Prefer the LaTeX source and also compile it when possible, because page count and fonts are only reliable in the PDF.
2. **The venue rules:** look for `cfp.md` in the paper folder. If it's missing, ask the user for the CFP link or text, or search the web for the venue's current author guidelines. Record the URL and the date you checked it.
3. **Review model:** single-blind, double-blind or open. If the CFP is unclear, ask.

Never assume rules from memory. Every limit you check against must come from the CFP or guidelines you found, and you must quote it.

## Checklist

### A. Template and format
- IEEE: `\documentclass[...]{IEEEtran}` with the right option (`conference`, `journal`, `compsoc`, etc.). ACM: `\documentclass[...]{acmart}` with the right format (`sigconf`, `acmsmall`, `manuscript`) and `review` / `anonymous` options as required.
- No forbidden margin, font-size or spacing hacks (`\vspace` abuse, `\small` on body text, `geometry` changes, negative `\baselineskip`).
- Paper size (US Letter vs A4) as required.

### B. Length
- Page count from the compiled PDF, both main text and total (with references and appendices), against the CFP limits. Note whether references count toward the limit.
- Word limits for the abstract and for magazine-style venues (for example IEEE Internet Computing counts words, references and figures).
- Number of figures, tables and references, if capped.

### C. Anonymization (double-blind only)
- No author names, affiliations, emails, acknowledgments or funding in the submission.
- Self-citations written in the third person ("Smith et al. [3] showed"), not "our previous work [3]".
- No identifying URLs (GitHub repos, company domains). Suggest anonymous.4open.science or similar.
- No company or product names that identify the authors (for example internal system names). Flag each one.
- PDF metadata: check `\hypersetup{pdfauthor=...}` and the PDF Author/Creator fields.
- Figure files: flag embedded logos, screenshots with usernames, and file paths.

### D. Required metadata
- IEEE: Index Terms / keywords present; author bios and photos only if the journal requires them at this stage.
- ACM: CCS concepts (`\begin{CCSXML}` + `\ccsdesc`), keywords, `\acmConference` / `\setcopyright` placeholders as the venue requires.
- Abstract within the length limit and without citations, if the venue forbids them.

### E. References
- Style matches the venue (IEEE numeric `IEEEtran.bst`; ACM `ACM-Reference-Format.bst`).
- No `[?]` or undefined references in the compiled output. Check the `.log` for warnings.
- DOIs present where the venue expects them. Hand off detailed checking to the **citation-verifier** skill in this toolkit.

### F. Figures and tables
- Fonts embedded (on the PDF, run `pdffonts`; all fonts should show `emb yes`, with no Type 3 if the venue forbids them).
- Figures readable at print size: axis-label text about 8pt or larger at final width; vector formats preferred.
- Figures understandable in grayscale if the venue prints in black and white.
- Every figure and table referenced in the text, captions in the correct position (IEEE: table captions above, figure captions below).

### G. Policy items
- **AI-use disclosure:** IEEE and ACM require disclosing substantial AI-generated content (usually in the acknowledgments or a dedicated section). Check that it's present, or flag that it's needed. Quote the venue's current policy wording if you can find it.
- Ethics, data availability and artifact statements, if required.
- Conflict-of-interest or prior-publication declarations (for example an arXiv preprint policy).
- Employer clearance reminder: if the paper uses data from the authors' employer, remind the user to get publication approval.

### H. Submission system
- Note what the submission form needs beyond the PDF (abstract text, topics, conflicts, supplementary material limits, PDF eXpress for IEEE camera-ready).

## How to run it

1. Locate and read the paper and the CFP. Compile with `latexmk -pdf` if a TeX install is available. If it isn't, check the PDF the user provides, or say clearly which checks were skipped.
2. Go through sections A–H. For each item, record **Pass / Fail / Warning / N/A** with evidence (file and line, page number, or quoted rule).
3. Fix nothing unless the user asks. Offer to apply the mechanical fixes (class options, CCS block, anonymizing self-citations) as a follow-up.

## Output

Write `compliance-report.md` in the paper folder, structured as:

```
# Venue Compliance Report — <paper> → <venue>
Checked: <date> | Rules source: <URL(s)> | Review model: <double-blind/...>

## Summary
<N> blocking issues, <M> warnings. Ready to submit: Yes/No

## Blocking issues
| # | Rule (quoted) | Location | Problem | Fix |

## Warnings
| # | ... |

## Passed checks
<compact list>

## Not checked (and why)
```

Then give the user a 2–3 sentence summary in chat: ready or not, the top blocking issues, and where the report is.
