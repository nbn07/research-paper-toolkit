---
name: "venue-compliance"
description: "Audit a paper (LaTeX or PDF) against the target venue's call for papers and author guidelines before submission — IEEE, ACM, Springer, Elsevier, and NeurIPS/ICML/ICLR-style ML venues. Trigger on requests like 'is this ready to submit', 'check the page limit', 'did I anonymize this correctly', or 'does this meet the CFP requirements' — checks formatting, length, anonymization, required metadata, references, figures, and policy items against the venue's actual rules, not generic defaults."
---

# Venue Compliance Audit (IEEE / ACM / Springer / Elsevier / NeurIPS-style)

Check a paper against the target venue's submission rules and report every violation with its exact location and a fix. Do not rewrite content or judge scientific merit; that is the review skills' job.

## Inputs

1. **The paper:** `main.tex` (plus any `\input` files, `refs.bib` and `figures/`) or the compiled PDF. Prefer the LaTeX source and also compile it when possible, because page count and fonts are only reliable in the PDF.
2. **The venue rules:** look for `cfp.md` in the paper folder. If it's missing, ask the user for the CFP link or text, or search the web for the venue's *current* author guidelines (and, for a journal, its specific "Guide for Authors" — publisher-wide pages are a starting point, not the final word). Record the URL and the date you checked it.
3. **Review model:** single-blind, double-blind or open. If the CFP is unclear, ask — don't assume one publisher has one dominant convention. Springer and Elsevier in particular set this per venue/journal, not publisher-wide.

Never assume rules from memory. Every limit you check against must come from the CFP or guidelines you found, and you must quote it. This matters even more for the details below marked "confirm from the CFP" — style-file versions, checklist wording, and numeric limits change yearly or per-venue, so treat anything here that looks like a fixed fact as a starting point for your search, not a substitute for it.

## Checklist

### A. Template and format
- **IEEE:** `\documentclass[...]{IEEEtran}` with the right option (`conference`, `journal`, `compsoc`, etc.).
- **ACM:** `\documentclass[...]{acmart}` with the right format (`sigconf`, `acmsmall`, `manuscript`) and `review`/`anonymous` options as required.
- **Springer:** `llncs.cls` for LNCS and most Springer CS proceedings; `sn-jnl.cls` for Springer Nature journals — these are different templates, so confirm which type of venue this is before checking the class. Treat any specific class *version* as something to confirm from the venue, not assume.
- **Elsevier:** `elsarticle.cls`. The bibliography style (`elsarticle-num`, `elsarticle-harv`, etc.) is set by the *specific target journal*, not fixed across Elsevier — check that journal's Guide for Authors.
- **NeurIPS/ICML/ICLR-style:** the venue's official style file for the *current* submission year (for example "the NeurIPS 2026 style file") — these are re-released yearly with real changes, so confirm the paper uses the current one rather than an old cached copy, and never hardcode a filename/version as if it's stable.
- No forbidden margin, font-size or spacing hacks (`\vspace` abuse, `\small` on body text, `geometry` changes, negative `\baselineskip`) for any venue.
- Paper size (US Letter vs A4) as required.

### B. Length
- Page count from the compiled PDF, both main text and total (with references and appendices), against the CFP limits. Note whether references count toward the limit.
- **NeurIPS/ICML/ICLR-style:** the mandatory paper checklist (see section D) is typically placed after the references and does **not** count toward the main page limit — but confirm this from the current CFP, since the exact rule has changed across years for this venue family.
- Word limits for the abstract and for magazine-style venues (for example IEEE Internet Computing counts words, references and figures).
- Number of figures, tables and references, if capped.

### C. Anonymization (double-blind only)
- No author names, affiliations, emails, acknowledgments or funding in the submission.
- Self-citations written in the third person ("Smith et al. [3] showed"), not "our previous work [3]".
- No identifying URLs (GitHub repos, company domains). Suggest anonymous.4open.science or similar.
- No company or product names that identify the authors (for example internal system names). Flag each one.
- PDF metadata: check `\hypersetup{pdfauthor=...}` and the PDF Author/Creator fields.
- Figure files: flag embedded logos, screenshots with usernames, and file paths.
- **NeurIPS/ICML/ICLR-style:** anonymization requirements typically extend explicitly to supplementary material and submitted code, not just the main PDF — check the current CFP for exactly what that covers.
- **Springer/Elsevier:** don't assume a review model — some Springer/Elsevier conferences and journals are single-blind, some double-blind, and it's set per venue. Get this from the specific CFP or Guide for Authors, not from "how Springer/Elsevier usually works."

### D. Required metadata
- **IEEE:** Index Terms / keywords present; author bios and photos only if the journal requires them at this stage.
- **ACM:** CCS concepts (`\begin{CCSXML}` + `\ccsdesc`), keywords, `\acmConference` / `\setcopyright` placeholders as the venue requires.
- **Elsevier:** a CRediT author statement is generally *encouraged, not mandatory* at the publisher level — check whether the specific target journal requires it. "Highlights" (a short list of bullet points, typically 3–5, with a per-bullet character limit) and a graphical abstract are required by some Elsevier journals and not others — get the exact count/limit from that journal's own Guide for Authors, never assume a number. A Declaration of Competing Interest is generally expected; get the exact required wording from the target journal.
- **Springer:** no publisher-wide extra metadata block confirmed — check the specific venue's instructions for authors.
- **NeurIPS/ICML/ICLR-style:** a completed paper checklist is mandatory (self-reporting on reproducibility, transparency, research ethics, and societal/broader impact) — the exact categories have changed across years for this venue family (for example, some years require a separate "Limitations" section or a "Broader Impact" statement that later got folded into the checklist), so pull the current checklist items from this year's CFP rather than assuming last year's categories.
- Abstract within the length limit and without citations, if the venue forbids them, for any venue.

### E. References
- Style matches the venue: IEEE numeric (`IEEEtran.bst`); ACM (`ACM-Reference-Format.bst`); Springer LNCS (commonly `splncs04.bst`, confirm for the specific venue); Elsevier (`elsarticle-num` or `elsarticle-harv`, set by the target journal); NeurIPS/ICML/ICLR-style (no single mandated style, but must be internally consistent and match what the venue's template expects).
- No `[?]` or undefined references in the compiled output. Check the `.log` for warnings.
- DOIs present where the venue expects them. Hand off detailed checking to the `citation-verifier` skill.

### F. Figures and tables
- Fonts embedded (on the PDF, run `pdffonts`; all fonts should show `emb yes`, with no Type 3 if the venue forbids them).
- Figures readable at print size: axis-label text about 8pt or larger at final width; vector formats preferred.
- Figures understandable in grayscale if the venue prints in black and white.
- Every figure and table referenced in the text, captions in the correct position (IEEE: table captions above, figure captions below; confirm the convention for other venues from their style guide).
- **Elsevier:** if a graphical abstract is required by the target journal, check it against that journal's size spec (a commonly seen baseline is roughly 531×1328 px, legible at about 5×13 cm — but get the exact current spec from the journal, since it can differ).

### G. Policy items
- **AI-use disclosure — this varies meaningfully by publisher, so don't use one wording for all of them:**
  - *IEEE/ACM:* both generally require disclosing substantial AI-generated content (usually in the acknowledgments or a dedicated section). Quote the venue's current wording if you can find it.
  - *Springer:* disclosure is required; low-risk assistive use (e.g., light copyediting) is generally permitted with disclosure, while undisclosed use of an LLM to generate core research reasoning is not. Where exactly to place the disclosure is set by the *specific journal's* Instructions for Authors, not a single Springer-wide location — check there.
  - *Elsevier:* requires a **separate, explicitly labeled declaration** (commonly titled "Declaration of generative AI and AI-assisted technologies in the manuscript preparation process"), distinct from the Methods/Acknowledgments section, naming the tool, its purpose, and the extent of human oversight. Routine grammar/spelling tools are typically exempt; substantive AI-assisted rewriting is not. If AI was used in the research process itself (data analysis, generated visualizations), that's disclosed separately in the Methods section, not folded into this declaration.
  - *NeurIPS/ICML/ICLR-style:* check the current CFP specifically — policy here is less publisher-standardized and more likely to be stated (or not) in that year's call. Don't assume it mirrors IEEE/ACM or Elsevier's approach.
- Ethics, data availability and artifact statements, if required.
- Conflict-of-interest or prior-publication declarations (for example an arXiv preprint policy).
- Employer clearance reminder: if the paper uses data from the authors' employer, remind the user to get publication approval.

### H. Submission system
- Note what the submission form needs beyond the PDF (abstract text, topics, conflicts, supplementary material limits, PDF eXpress for IEEE camera-ready).
- **NeurIPS/ICML/ICLR-style:** submission and review normally run through OpenReview. All authors typically need an OpenReview profile created by the submission deadline, and a profile without an institutional email can face a moderation delay (sometimes up to roughly two weeks) — flag this to the author early in the process, not at submission time, since it can block submission through no fault of the paper itself.
- **Springer/Elsevier:** typically run through a publisher-specific portal (for example Editorial Manager) rather than OpenReview — note whatever the specific venue's CFP says about its submission system.

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
