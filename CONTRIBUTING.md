# Contributing

Thanks for your interest in contributing to Emotional Intelligence Prompting. This project translates research into practical tools — contributions that maintain that connection are welcome.

---

## The One Rule

**Every principle, pattern, or recommendation must cite its research basis.**

This project is grounded in Anthropic's ["Emotion Concepts and their Function in a Large Language Model"](https://transformer-circuits.pub/2026/emotions/index.html) paper. Advice that isn't traceable to specific research findings doesn't belong here. There are plenty of places for general prompting tips — this repo is specifically about what the emotion concepts research tells us.

If you want to contribute something that extends beyond the paper's findings, label it clearly as extrapolation and explain the reasoning. Don't present speculation as established finding.

---

## What to Contribute

### High-Value Contributions

- **New scenarios** in `examples/scenarios/` — real-world before/after examples showing EIP principles in action. The more specific and realistic, the better.
- **Domain-specific system prompts** in `examples/system-prompts/` — prompts tuned for specific professional contexts (legal, medical, education, finance, etc.).
- **Domain-specific CLAUDE.md configs** in `examples/claude-code-configs/` — configs tuned for specific project types.
- **Corrections** — if something in the docs misrepresents the research, fix it. Accuracy matters more than volume.
- **Clarity improvements** — if something is confusing, unclear, or could be said more simply, improve it.

### Welcome But Review-Heavy

- **New principles or anti-patterns** — these need strong research grounding. Open an issue to discuss before writing.
- **Template improvements** — changes to the builder templates affect everyone's output. Propose changes in an issue first.
- **Structural changes** — reorganizing files, changing the information hierarchy. Discuss first.

### Not a Fit

- **General prompting tips** not grounded in the emotion concepts research.
- **Speculation about AI consciousness** — the paper doesn't go there and neither do we.
- **Claims about models other than Claude** — the research is on Claude Sonnet 4.5. Don't generalize.
- **Marketing language** — keep the tone direct and practical, not promotional.

---

## Writing Standards

These apply to all prose in the repo:

- **Direct and clear.** Lead with the practical takeaway, then explain the mechanism.
- **One idea per paragraph.** If a paragraph has two ideas, split it.
- **Concrete over abstract.** Use examples. Show, don't just tell.
- **No padding.** If something can be said in 2 sentences, don't use 5.
- **No overclaiming.** The paper doesn't prove consciousness. Don't imply it. Stay within what the research actually demonstrates.
- **Cite the research.** Every prescriptive claim should trace back to a specific finding from the paper.

---

## How to Contribute

### Small Changes (typos, clarifications, corrections)

1. Fork the repo.
2. Make the change.
3. Open a pull request with a brief description of what you changed and why.

### New Content (scenarios, system prompts, configs)

1. Check existing content to avoid duplication.
2. Fork the repo.
3. Write your contribution following the existing format for that content type.
4. Ensure every recommendation cites its research basis.
5. Open a pull request with:
   - What you're adding
   - Which research findings it's based on
   - Any areas you're uncertain about

### Larger Changes (new docs, structural changes, new principles)

1. Open an issue first to discuss the proposal.
2. Wait for feedback before writing.
3. This saves everyone time if the direction needs adjustment.

---

## Style Guide for Examples

If you're contributing system prompts, CLAUDE.md configs, or scenarios:

- **System prompts must be self-contained.** Copy-paste and go. No external dependencies or references the user needs to look up.
- **CLAUDE.md configs must be drop-in.** Save as `CLAUDE.md` in a project root and it works.
- **Scenarios need realistic situations.** Not toy examples. Real tasks, real failure modes, real conversations.
- **Before/after examples should show the same task.** The variable is the prompting approach, not the task complexity.
- **Explain the mechanism.** Don't just show that EIP works better — explain which principles drive the difference and why, citing the research.

---

## Code of Conduct

Be constructive, be specific, be honest. The same principles we apply to AI interaction apply to human interaction: grant permission to fail, acknowledge difficulty, collaborate rather than command.

If someone's contribution needs work, explain what's missing and how to fix it. If you disagree with an approach, engage with the reasoning rather than dismissing it.

---

## Questions?

Open an issue. We'd rather answer a question than clean up a misaligned contribution.
