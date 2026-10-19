---
name: "ieee-fresh-eyes-paper-review"
description: "Use before submitting a research paper, or whenever asked for an independent, blind, or 'fresh-eyes' review, to simulate reviewers who never saw the drafting conversation and so catch what the author can't see anymore. Trigger on requests like 'review this before I submit', 'what would Reviewer 2 say', 'get an outside opinion on this paper', or 'simulate peer review' — run this before ieee-final-review for the deepest pre-submission check."
---

# Fresh-eyes paper review

Purpose: catch what the author (and any assistant that helped write the paper) cannot see because they already know what the text *means*. Reviewers must read only the final paper, exactly as a program-committee member would.

## 1. Prepare an isolated input
- Take the final PDF (or exported text) only. Extract text with `pdftotext -layout paper.pdf paper.txt`; keep the PDF path too so reviewers can look at tables and figures.
- Collect the venue's call for papers facts (page limit, review criteria, double-blind rules) in 3-5 lines.
- Do NOT give reviewers the conversation, source files, revision logs, earlier versions, or the authors' intentions.

## 2. Run independent reviewers in parallel
Launch one subagent per persona in a single message (Agent tool), so none sees another's output. If subagents aren't available in this session, do each pass yourself sequentially, rereading only the paper (not your own prior pass) each time, and say in the report that this was a manual substitute for independent parallel review. Each prompt contains only: the persona, the venue facts, the paper path, and the output format below, plus: "Read only this paper. Do not open any other file. If something is unclear, report it as unclear; do not guess what the authors meant."

Default personas (adapt to the paper's domain):
1. **Domain practitioner**: an engineer who would deploy the method in their own lab or product. Asks: how does this work in my setting, what manual steps are hidden, what breaks at scale, what would I need to adopt it?
2. **Skeptical program-committee member** at the target venue: gives a score (strong reject to strong accept), the top three reasons, the most likely rejection argument, and missing baselines or related work.
3. **Methods expert**: statistics, ML or formal methods as relevant. Checks assumptions, leakage between fitting and evaluation, post-hoc choices, whether the conclusions follow from the numbers.
4. **Newcomer reader** (a strong PhD student outside the subfield): marks every place they got confused or had to reread, with quotes.
Optional 5. **Artifact reviewer**: lists every promise the paper makes about the artifact or data and whether a reader could check it.

Required output format for each reviewer:
- Questions a reader would ask (each with section and short quote)
- Points of confusion (quote and why)
- Claims they do not believe, or believe are overstated
- Missing comparisons, threats or related work
- Suggestions for future work they would expect mentioned
- Severity for each item: blocking / major / minor

## 3. Consolidate and verify (main agent)
- Merge duplicates across reviewers; note how many raised each point.
- For every item, check the paper and classify it:
  - **Real gap**: the paper does not answer it.
  - **Answered but hard to find**: give the section and quote; propose a pointer or a clearer sentence.
  - **Out of scope or future work**: propose at most one sentence, or nothing.
  - **Reviewer error**: explain briefly.
- For each proposed fix, give the exact wording, where it goes, its word count, and what could be cut to stay within the page limit.

## 4. Report to the user
- Start with the predicted score range and the single most likely rejection reason.
- Then a table: item, raised by, classification, proposed fix, words.
- Do not edit the paper; let the user choose which fixes to apply.
- State plainly that these are simulated reviewers: they reduce author blindness but do not replace human domain readers.
