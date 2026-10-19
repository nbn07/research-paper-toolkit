---
name: "ieee-research-paper-workflow"
description: "Use when planning, researching, drafting, reviewing, submitting or revising a research paper for an IEEE or ACM venue, to follow a staged workflow from venue choice to rebuttal. Orchestrates the other skills in this toolkit (ieee-citation-verifier, ieee-venue-compliance, ieee-fresh-eyes-paper-review, ieee-final-review) at the right stage. Trigger on requests like 'help me pick a venue for this paper', 'draft Section III', 'is this ready to submit', or 'turn these reviews into a rebuttal' — even if the user doesn't name IEEE or ACM explicitly."
---

# Research Paper Workflow (IEEE / ACM)

A staged workflow for taking a paper from idea to submission and revision. Work out which stage the user is in, do that stage well, and suggest the next one. Don't run every stage unprompted.

## Ground rules (always apply)

1. **Never invent citations, results or system facts.** Every reference must come from a search tool or from the user. Technical facts about the authors' own system and numbers come only from their notes, data or answers. If something's missing, ask; don't fill the gap.
2. **Flag unsupported claims.** If a sentence claims more than the data or cited work shows, say so.
3. **Be a critic, not a cheerleader.** Point out weak novelty, missing baselines, overclaiming and threats to validity.
4. **Work one section at a time** unless the user asks otherwise. Keep to the page or word budget.
5. **Edit the user's files in place** (`main.tex`, `refs.bib`) only when asked. Otherwise propose a diff or write a new file.
6. **Keep terminology consistent.** Use the project's own names for systems and concepts exactly as the user writes them.
7. **Employer and confidential data.** If the work uses data from the authors' employer, remind them once to get publication or legal approval, and suggest anonymizing internal names and metrics.
8. **AI disclosure.** IEEE and ACM require disclosing substantial AI-generated content (usually in the acknowledgments), and AI can't be an author. Remind the user before submission, and check the venue's current wording.
9. **Research tools vary by session.** Steps below name Consensus, alphaXiv and SciSpace because they're excellent when connected — but don't assume they are. If a named tool isn't available, fall back to general web search/fetch for the same purpose (finding peer-reviewed sources, arXiv full text, or current CFPs) rather than skipping the step or filling the gap from memory.

## Getting context first

Before substantial work, check for (and read if present):
- Project docs or status files for this paper (for example `*-submission-status.md`, `*-venue-shortlist.md`, review notes).
- The paper folder in the user's linked folder. The expected layout is:
  ```
  <PaperName>/  main.tex  refs.bib  figures/  data/  notes/  cfp.md
  ```
- If the target venue, audience, length or co-authors are unknown and matter for the task, ask in one short question.

## Stages

### 1. Venue selection
Compare candidate IEEE/ACM venues on scope fit, deadlines, page or word limits, review model (single- or double-blind), acceptance rate, indexing, and turnaround. Search the web for current CFPs; never quote deadlines from memory. Output a ranked shortlist table with sources, and save the chosen venue's guidelines as `cfp.md`.

### 2. Literature review and gap
Search with Consensus (peer-reviewed), alphaXiv (arXiv full text) and SciSpace if connected; otherwise use web search for the same purpose. Build a comparison table: work, problem, method, dataset or setting, metrics, limitations. Synthesize the gap the paper fills. Prefer recent work (past 3–5 years) plus seminal papers. Add verified BibTeX entries to `refs.bib` only with the user's OK.

### 3. Contribution framing
Sharpen the problem statement, research questions and 3–4 contribution bullets. Stress-test novelty against the closest related work and say plainly if it's weak.

### 4. Outline
Plan the sections with a page or word budget against the venue limit. A typical structure: Introduction, Background / Related Work, System or Method, Evaluation (setup, results, discussion), Threats to Validity / Limitations, Conclusion. Adapt this for magazine venues (for example IEEE Internet Computing or IEEE Software) that favor a practitioner style and fewer references.

### 5. Experiments, analysis and figures
Analyze the user's data with appropriate statistics (effect sizes, confidence intervals, significance tests where they apply). Make figures as vector PDFs sized to the column (IEEE single column about 3.5 in, double about 7.16 in), with fonts of 8pt or larger at final size and readable in grayscale. Don't change or drop data points without saying so.

### 6. Drafting
Write in LaTeX with the venue class (IEEEtran or acmart), one section at a time, from the user's notes and results. Use formal academic English, active voice where natural, and no marketing language. Mark any placeholder the user must fill as `\todo{...}`.

### 7. Citation verification
Hand off to the **ieee-citation-verifier** skill in this toolkit.

### 8. Venue compliance
Hand off to the **ieee-venue-compliance** skill in this toolkit.

### 9. Independent review
Run **ieee-fresh-eyes-paper-review** first (independent simulated reviewers who never saw the drafting conversation), then **ieee-final-review** (a fast deterministic checklist pass covering contribution, soundness, baselines, citation ethics, structure, visuals, and language). Bring the combined findings back as a single prioritized fix list, noting which reviewer or checklist item raised each issue.

### 10. Submission package
Polish the title (offer 3 options) and abstract (within the limit). Prepare the keywords, IEEE Index Terms or ACM CCS concepts, the cover letter if needed, suggested reviewers if asked for, the AI-use disclosure, and a final checklist of the submission form's fields.

### 11. Reviews, rebuttal and camera-ready
Turn the reviews into a response matrix: reviewer, comment, category (major, minor or clarification), planned change, and location in the paper. Draft a polite, specific rebuttal or response letter. For camera-ready, restore author information, add acknowledgments and funding, apply the copyright block, and run IEEE PDF eXpress or the ACM TAPS checks as the venue requires.

## Prompts that work well (share these with the user when they're helpful)

- "This is for <venue>, <audience>, <length>. Here's Section N. Tighten it to about X words without losing claims."
- "Act as a skeptical Reviewer 2. What would make you reject this?"
- "Find 2022–2026 work on <topic>. Tabulate method, dataset, metrics, limitations. What gap remains?"
- "Analyze data/<file>.csv. Compare baseline vs ours with significance tests and make a 3.5-inch figure."
- "Run ieee-citation-verifier, then ieee-venue-compliance, then ieee-fresh-eyes-paper-review, then ieee-final-review."

## End of each session

Offer to update the paper's status doc with: the current stage, decisions made, open issues and owners, and next steps with dates. That way the next session starts with full context.
