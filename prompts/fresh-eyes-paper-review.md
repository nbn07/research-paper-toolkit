You are simulating independent peer review of a research paper, for an IEEE or ACM venue. The whole point is to catch what I — the author — can no longer see, because I already know what my own text is supposed to mean. Review the paper the way an actual program-committee member would: cold, with no context beyond what I give you here.

**Important — start this in a brand-new conversation if at all possible.** If this is the same chat where I drafted the paper, you already have context that a real reviewer wouldn't have (my intentions, earlier drafts, things I explained to you along the way), and that will make this review far less useful. If I'm continuing in the same chat anyway, explicitly set aside everything you know about my intentions for this paper and review only the text I'm about to paste, exactly as a stranger would read it.

## What I'll give you

- The final (or near-final) paper text, as completely as I can paste it — ideally including figure/table captions, since you can't see images directly unless I describe them or you can view an attached file.
- A few lines on the venue's call for papers: page limit, review criteria, whether it's double-blind.

Do not ask me what I "meant" by something, and don't let me explain it to you mid-review — if something is unclear from the text alone, that unclarity IS the finding. Note it as confusion, not as a question for me to answer before you continue.

## Step 1: Reviewer personas

Review the paper through each of these lenses. If you can do genuinely separate passes (e.g., I ask you to run them one at a time, rereading only the paper and not your own earlier persona's notes), do that — it keeps the personas from blurring into each other. If you can only do one pass covering all of them, say so, and be deliberate about keeping each persona's voice distinct rather than blending them into one generic "here are some issues" list.

1. **Domain practitioner** — an engineer who would actually try to deploy or build on this method. Asks: how would this work in my setting, what manual steps are hidden, what breaks at scale, what would I need in order to adopt this?
2. **Skeptical program-committee member** at the target venue — gives a score (strong reject to strong accept) with the top three reasons, the single most likely rejection argument, and any missing baselines or related work.
3. **Methods expert** (statistics, ML, or formal methods, whichever fits the paper) — checks assumptions, leakage between fitting and evaluation, post-hoc choices, and whether the stated conclusions actually follow from the reported numbers.
4. **Newcomer reader** — a strong student outside this specific subfield. Marks every place they got confused or had to reread, quoting the exact passage.
5. *(Optional)* **Artifact reviewer** — lists every promise the paper makes about code/data/artifacts, and whether a reader could actually verify each one from what's provided.

For each persona, produce:
- Questions a reader would ask (with the section and a short quote)
- Points of confusion (quote + why)
- Claims they don't believe, or think are overstated
- Missing comparisons, threats to validity, or related work
- Suggestions for future work they'd expect to see mentioned
- A severity for each item: blocking / major / minor

## Step 2: Consolidate

Once all personas are done:
- Merge duplicate points raised by more than one persona, and note how many raised each one.
- For every item, classify it:
  - **Real gap** — the paper genuinely doesn't address this.
  - **Answered but hard to find** — point to where it IS answered, and suggest a clearer pointer or sentence.
  - **Out of scope / future work** — at most one sentence needed, or nothing.
  - **Reviewer error** — explain briefly why the concern doesn't actually apply.
- For proposed fixes, give the exact wording you'd suggest, roughly where it goes, its approximate length, and what could be trimmed elsewhere to stay within the page/word budget.

## Step 3: Report back to me

- Lead with the predicted score range and the single most likely reason this paper would be rejected.
- Then a table: item | raised by (which persona) | classification | proposed fix | approx. length.
- Don't edit the paper yourself — let me decide which fixes to apply.
- Be explicit that these are simulated reviewers: useful for reducing my own blindness to the text, but not a substitute for an actual human expert in this specific subfield reading it before I submit.
