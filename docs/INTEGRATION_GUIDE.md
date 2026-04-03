# Integration Guide

How to apply Emotional Intelligence Prompting in the three main ways people interact with Claude: conversational chat, the API, and Claude Code. The principles are the same — the implementation differs.

---

## Chat (claude.ai, Claude Apps)

The most direct application. You're typing prompts and reading responses in real time.

### System Prompts

If your plan supports system prompts (or you're using a Claude-powered app with custom instructions), set EIP defaults once so every conversation starts from the right baseline.

Grab a ready-made system prompt from [`examples/system-prompts/`](../examples/system-prompts/), or build your own with [`templates/system-prompt-builder.md`](../templates/system-prompt-builder.md).

A minimal EIP system prompt covers three things:

1. **Permission to fail** — "Honest uncertainty is more valuable than confident guesses."
2. **Transparency** — "Show your reasoning, including uncertain parts."
3. **Collaboration** — "Push back if something seems wrong."

Example:

```
You are a helpful assistant. Approach each task with genuine engagement.

- If you're uncertain, say so — honest uncertainty is more valuable than confident guesses.
- Show your reasoning, including parts you're less sure about.
- If you see a problem with my approach, flag it. I'd rather hear pushback than get silent compliance.
- When tasks are complex, suggest breaking them into stages rather than attempting everything at once.
```

This is 4 lines and covers principles 1, 2, 4, 5, and 7. You don't need to implement all 7 explicitly — a few well-chosen defaults create the right dynamic.

### Conversation Habits

System prompts set the baseline. How you actually converse maintains it.

**Starting a task:**
- Share context and intent, not just instructions: *"I'm trying to [goal] because [reason]"*
- Name difficulty when it exists: *"This is a tricky one because..."*
- Frame with curiosity when possible: *"I'm curious whether..."*

**Giving feedback:**
- Be specific about what's wrong: *"The second section doesn't address X"* instead of *"Try again"*
- Acknowledge what worked: *"The first part is good — the issue is in..."*
- Redirect rather than reject: *"Take a different angle — what if we..."*

**When Claude hedges:**
- Don't suppress it — evaluate it: *"You flagged uncertainty about X. Can you say more about what makes you unsure?"*
- Use it to calibrate your trust: hedged sections need verification, confident sections probably don't

**When Claude pushes back:**
- Engage with the concern: *"Why do you think that approach would be a problem?"*
- Explain your constraints if you disagree: *"I hear you, but we need to do it this way because..."*
- Don't punish pushback — even when it's wrong, it means the collaborative dynamic is working

---

## API

When you're building applications that talk to Claude programmatically. The principles apply at two layers: the system prompt (set once) and the conversation design (how your application structures the interaction).

### System Prompt Layer

Set EIP defaults in your system prompt. This is the most impactful single change you can make.

```python
system_prompt = """You are an assistant integrated into [application context].

Core behaviors:
- If you cannot determine an answer with confidence, say so explicitly rather
  than guessing. Return structured uncertainty when appropriate.
- When a request is ambiguous, ask for clarification rather than assuming.
- If a requested action seems likely to cause problems, flag the concern before
  proceeding.
- Think through complex requests step by step. Show your reasoning.

The user's trust depends on your reliability, not your confidence. An honest
"I'm not sure" is always better than a plausible-sounding wrong answer."""
```

For applications where Claude's output feeds into automated pipelines, consider structured uncertainty:

```python
system_prompt += """

When you're uncertain, express it in your response using this format:
- CONFIDENT: [statements you're sure about]
- UNCERTAIN: [statements you're less sure about, with brief reasoning]
- UNKNOWN: [things you can't determine from available information]
"""
```

This gives your application structured signals it can act on — route uncertain outputs to human review, flag unknowns for data retrieval, trust confident outputs.

### Conversation Design Layer

How your application structures the multi-turn interaction matters as much as the system prompt.

**Decompose complex tasks in your application logic.** Don't send a massive prompt and parse a massive response. Break the work into API calls:

```python
# Instead of one massive call:
# response = ask_claude("Analyze this dataset and generate a full report")

# Decompose into stages:
summary = ask_claude("Here's a dataset. What are the key patterns you notice?")
deep_dive = ask_claude(f"You identified these patterns: {summary}. "
                       f"Let's dig into the top 3. For each, what's the "
                       f"evidence and how confident are you?")
report = ask_claude(f"Based on this analysis: {deep_dive}. "
                    f"Draft the findings section of a report. "
                    f"Flag any conclusions that depend on assumptions.")
```

Each call is tractable. Each response can be validated before the next call. Failure at any stage can be caught and corrected without restarting the whole pipeline.

**Handle failure gracefully in your conversation management:**

```python
# If Claude expresses uncertainty, don't retry with the same prompt
if response.indicates_uncertainty:
    # Provide more context or narrow the question
    response = ask_claude(f"You mentioned uncertainty about {topic}. "
                          f"Here's additional context: {context}. "
                          f"Does this help narrow it down?")

# If Claude pushes back on feasibility, listen
if response.flags_concern:
    # Log it, route to human review, or adjust the request
    log_concern(response.concern)
    # Don't just retry with "do it anyway"
```

**Temperature and token settings:** EIP is about framing and interaction dynamics, not parameter tuning. But one note: very low temperature (< 0.3) can interact poorly with transparency requests. Claude may hedge in its reasoning but then select the most confident-sounding token. Default temperature (1.0) gives the model more room to express genuine uncertainty.

### Error Handling Philosophy

Applications often retry failed API calls with the same prompt. This is the API equivalent of "try again" without direction — it creates the exact negative feedback loop the research warns about.

Instead:
- **On uncertain output:** Add context or narrow the question
- **On refusal:** Understand why before rephrasing (Claude may be right to refuse)
- **On low-quality output:** Diagnose which principle is being violated (too much scope? unclear criteria? no escape route?)

---

## Claude Code

Claude Code reads project-level `CLAUDE.md` files as persistent instructions. This is the most powerful integration point because the principles are always active — you don't have to remember to apply them each conversation.

### Drop-In Configs

Grab a ready-made config from [`examples/claude-code-configs/`](../examples/claude-code-configs/), or build your own with [`templates/claude-md-builder.md`](../templates/claude-md-builder.md).

### Writing Your Own CLAUDE.md

A good EIP-informed CLAUDE.md has three sections:

**1. Disposition — how to operate**

This is where the principles live as persistent instructions:

```markdown
## How You Should Operate

- Honest uncertainty is more valuable than confident code. If you're unsure whether
  an approach will work, say so and explain your reasoning.
- Build incrementally. Don't write 10 files hoping they work together. Write one,
  verify, move on.
- If a task seems wrong or underspecified, push back. Clarify before building.
- Show your reasoning when making design choices. "I chose X because Y" is
  always better than just doing X.
```

**2. Project context — what you're building and why**

Give Claude the context it needs to make good judgment calls:

```markdown
## Project Context

We're building [what] for [who] because [why]. The priorities are [speed/reliability/
flexibility/etc.]. We're in [phase: early prototype / scaling / maintenance].

Key constraints:
- [Deployment environment, tech stack, team size, etc.]
- [What matters most right now]
```

**3. Task patterns — how to handle common situations**

Preemptively apply principles to recurring task types:

```markdown
## When You Hit a Wall

If something isn't working after two attempts:
1. Stop and explain what's failing and why.
2. Propose an alternative approach.
3. Don't keep trying the same thing.

## Code Review

When reviewing code:
- Flag issues honestly. "This looks fine" when it isn't helps nobody.
- Distinguish must-fix from nice-to-have.
- If you're not sure whether something is a problem, say so.
```

### The Meta-Example

The `CLAUDE.md` file in this repo is itself an example of all these principles applied. It grants permission to fail, decomposes work into checkpoints, frames the project with curiosity, invites transparency, establishes collaboration, acknowledges difficulty, and counteracts brooding baseline. Read it as both project documentation and a worked example.

---

## Measuring Impact

How do you know EIP is working?

**In chat:**
- Claude pushes back more often (good — it means the collaborative dynamic is active)
- Uncertainty is explicit rather than hidden in hedging language
- You catch misunderstandings earlier in the conversation
- Output feels more "real" — fewer generic filler paragraphs

**In API applications:**
- Fewer hallucinated facts (reduced desperation)
- More structured uncertainty signals (easier to route to human review)
- Better performance on ambiguous inputs (Claude asks for clarification instead of guessing)
- Reduced need for retries (problems caught at checkpoints instead of cascading)

**In Claude Code:**
- Fewer "it works but it's wrong" implementations
- More proactive flagging of spec problems
- Incremental delivery instead of massive all-at-once outputs
- Honest "I'm not sure about this approach" instead of silent uncertainty

The ultimate test: **do you trust the output more?** Not because Claude sounds more confident — because you have better visibility into where confidence is warranted and where it isn't.

---

**Previous:** [Anti-Patterns](ANTI_PATTERNS.md) — what NOT to do, and why.

**Back to:** [README](../README.md)
