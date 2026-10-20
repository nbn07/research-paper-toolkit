# Prompt pack (for any AI assistant)

The `skills/` folder in this repo only works inside Claude Code or Claude Cowork, where `SKILL.md` files are auto-discovered. If you use a different assistant — ChatGPT, Gemini, Copilot Chat, a local model, anything with a "custom instructions" / "system prompt" box or just a plain chat window — use the files in this folder instead. They contain the same methodology, rewritten as plain instructions with no Claude-specific tool calls, file-writing, or skill hand-offs assumed.

## Files

| File | Use it for |
|---|---|
| `research-paper-workflow.md` | The overall process: venue selection → lit review → drafting → review → submission → rebuttal. Start here. |
| `citation-verifier.md` | Checking every entry in your bibliography against a real source. |
| `venue-compliance.md` | Auditing your paper against the actual target venue's CFP (length, format, anonymization, required metadata...). |
| `fresh-eyes-paper-review.md` | An independent, simulated multi-reviewer pass before you submit. |
| `final-review.md` | A fast, last-mile checklist pass right before submission. |

## How to use these

1. Open a **new** chat with your assistant for whichever task you're doing. Starting fresh matters most for `fresh-eyes-paper-review.md` — if you reuse the chat where you drafted the paper, the assistant already "knows" what you meant and can't review it with genuinely fresh eyes.
2. Paste the whole contents of the relevant file as your first message (or into the assistant's system-prompt / custom-instructions field, if it has one).
3. Follow it with your actual request and the paper content — e.g. paste your bibliography after `citation-verifier.md`, or your draft section after `research-paper-workflow.md`.
4. When the workflow prompt tells you to switch to another prompt for a stage (for example, "use the citation-verifier prompt now"), open a new chat with that file instead of expecting the same chat to switch roles — most assistants follow a role more reliably when it's the first thing in a fresh conversation.

## What's different from the Claude skills

These files assume nothing about what tools your assistant has:

- **No file reads/writes.** The Claude skills read and write files like `refs.bib`, `main.tex`, `cfp.md`, and report files like `citation-report.md`. These prompts instead ask for that content to be pasted into the chat, and produce their output as a markdown section in the reply — copy it into a file yourself if you want to keep it.
- **No named research tools.** The Claude skills can call Consensus, alphaXiv, SciSpace and general web search. These prompts just say "use your web search/browsing capability if you have one; otherwise ask the person to paste the source text or a link" — some assistants (an offline/local model, a chat app with browsing turned off) have none, and the prompts are written to degrade gracefully rather than invent sources.
- **No subagents.** The Claude fresh-eyes review runs several reviewer personas as separate parallel subagents so none can see another's take. Most chat assistants can't do that, so the prompt instead tells the assistant to run each persona as its own pass, rereading only the paper (never its own earlier pass) between personas, and to say plainly that this is a manual stand-in for true independent review.
- **No automatic hand-offs.** The Claude orchestrator skill calls the other four skills by name mid-conversation. These prompts instead tell you, in plain language, which other prompt file to switch to and when.

Everything else — the actual IEEE/ACM domain knowledge, the checklists, the "never invent a citation or a result" rule, the verdict categories — is unchanged.

## A note on capability

These prompts tell your assistant what to check and how to think about each stage; they can't give an assistant web access, file access, or subagents it doesn't have. If your assistant can't browse the web, it cannot verify a citation or look up a venue's current CFP — in that case it should say so and ask you to paste the source material, rather than guessing. If it answers confidently about a paper's existence or a venue's page limit without a source you gave it, don't trust that answer; that's exactly the failure mode this toolkit exists to prevent.
