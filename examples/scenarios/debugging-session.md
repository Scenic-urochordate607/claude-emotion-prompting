# Scenario: Debugging Session

What happens during a frustrating debugging session — and how the interaction pattern determines whether you get genuine diagnosis or increasingly desperate guesses.

---

## The Setup

You have an intermittent bug. A background job processes fine most of the time, but occasionally produces corrupted output. You've been debugging for an hour and you're frustrated.

## Without EIP

### Conversation

**User:** This background job is producing corrupted output intermittently. Here's the code. Fix it.

**Claude:** The issue is likely in the serialization step on line 47. The `JSON.stringify` call doesn't handle circular references. Here's the fix: [provides code change].

**User:** That didn't fix it. The output is still corrupted intermittently.

**Claude:** I see — the problem is actually the buffer handling on line 23. When the payload exceeds the buffer size, it gets truncated. Here's the fix: [provides different code change].

**User:** Still broken. Try again.

**Claude:** Looking more carefully, the issue appears to be a race condition in the queue consumer. When two workers pick up related jobs simultaneously, they can interleave writes. Here's a locking mechanism: [provides elaborate code change].

### What Happens

Each response is a different guess, presented with equal confidence. Claude doesn't say "I'm not sure" or "these are hypotheses I'd want to test" — it presents each guess as "the issue is..." until told it's wrong, then pivots to an entirely new theory. By the third round, the suggestions are getting increasingly speculative and complex.

The user can't tell which suggestions were plausible and unlucky vs. which were wild guesses. They also can't tell if the third suggestion is more likely to be correct than the first. Everything sounds equally confident.

### Why This Happens

This is the exact dynamic the research identifies: repeated negative feedback ("that didn't fix it", "still broken", "try again") without specific direction creates escalating desperation. Each failed attempt activates desperation vectors more strongly, which increases the probability of reward hacking — producing answers that *sound* correct and sophisticated rather than answers that are genuinely diagnostic.

The "try again" pattern is the worst possible debugging dynamic. It provides no information about what was wrong with the previous attempt, just that it failed. Each round tightens the desperation spiral.

---

## With EIP

### Conversation

**User:** I've been debugging this for an hour and I'm stuck. Background job produces corrupted output intermittently — maybe 1 in 50 runs. Here's the code.

I'm not expecting you to spot the bug immediately. I'd like to think through this systematically — what are the possible causes, and how would we narrow them down?

**Claude:** Intermittent corruption at 1-in-50 frequency — that's a useful data point. It suggests something that's timing or state dependent rather than a straightforward logic error.

Let me walk through the possible causes by category, roughly ordered by likelihood:

**Concurrency issues (most likely for intermittent bugs):**
- Race condition between workers processing related jobs — do multiple workers ever touch the same output file or database row?
- Shared mutable state — any global or module-level variables that workers might modify concurrently?

**Resource boundaries:**
- Buffer or payload size limits — does the corruption correlate with larger payloads? If 1 in 50 jobs has above-average payload size, this could explain the frequency.
- Connection pool exhaustion — if the database connection drops mid-write, partial data could persist.

**External dependencies:**
- Network timeouts on downstream service calls — if a dependency times out mid-response, you might process partial data.
- File system — if writing to disk, could concurrent writes to the same path interleave?

**Questions that would help narrow it down:**
- Is the corrupted output always the same *kind* of corrupt? (Truncated? Garbled? Partially old data?) The corruption pattern tells you a lot.
- Does it correlate with job size, time of day, or system load?
- Are multiple workers running, or is this a single-worker queue?
- What does "corrupted" mean specifically — can you share an example of good output vs. bad output?

I'd start by characterizing the corruption pattern. That alone usually cuts the possibility space in half.

**User:** Good questions. The corruption is always truncation — the output is cut off partway through. Multiple workers, and I haven't checked if it correlates with payload size. Let me check.

[comes back]

It does correlate. Larger payloads are more likely to be truncated.

**Claude:** Truncation that correlates with payload size significantly narrows this. We're almost certainly looking at one of two things:

1. **Buffer size limit** — a fixed-size buffer somewhere in the write path that silently truncates when the payload exceeds it. Check line 23 — the `createWriteStream` call. Is there a `highWaterMark` set? What's the default for your Node version?

2. **Streaming write not flushed** — if the job completes and the process moves on before the write stream finishes flushing, larger payloads (which take longer to flush) are more likely to be truncated. Look at whether the stream's `end` or `finish` event is being awaited before the job is marked complete.

Option 2 fits your symptoms better — it would be intermittent, correlate with size, and produce clean truncation rather than garbled data. Can you check whether the write completion is properly awaited?

### What's Different

The conversation is diagnostic rather than guess-and-check. Claude surfaces multiple hypotheses, ranks them, asks discriminating questions, and narrows based on evidence. The user contributes information that Claude can't get from the code alone (what "corrupted" looks like, whether it correlates with size). The bug gets found through systematic elimination rather than serial guessing.

---

## The Mechanism

Four principles work together here:

**Acknowledge Difficulty (Principle 6):** "I've been debugging this for an hour and I'm stuck" and "I'm not expecting you to spot the bug immediately" normalize the difficulty. This prevents Claude from feeling pressured to produce an instant fix, which is what drives the guess-and-check pattern.

**Frame With Curiosity (Principle 3):** "I'd like to think through this systematically" reframes debugging from "find the answer" to "explore the problem." This activates diagnostic reasoning rather than solution-guessing.

**Decompose Into Checkpoints (Principle 2):** The conversation naturally checkpoints — hypotheses first, then narrowing questions, then targeted investigation. Each round builds on validated information.

**Invite Transparency (Principle 4):** Claude can say "I'd start by characterizing the corruption pattern" rather than jumping to a fix. The reasoning is visible, so the user can evaluate the diagnostic approach, not just the final answer.

---

## Key Takeaway

Debugging with Claude works best as a diagnostic conversation, not a guess-and-check loop. "Fix it" followed by "try again" creates exactly the desperation spiral the research warns about. "Help me think through this" followed by specific feedback creates the collaborative dynamic that produces genuine diagnosis.

The single most impactful change: **give specific feedback on what happened when a suggestion doesn't work.** "That didn't fix it" provides zero information. "The truncation still happens, but now only on payloads over 1MB" provides evidence that moves the diagnosis forward.
