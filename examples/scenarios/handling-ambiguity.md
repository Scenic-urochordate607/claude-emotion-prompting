# Scenario: Handling Ambiguity

What happens when requirements are unclear — and how the interaction dynamics determine whether you get honest clarification or confident fabrication.

---

## The Setup

You're building a feature and the requirements are underspecified. You ask Claude to implement it.

## Without EIP

### Prompt

> Implement a caching layer for our API responses. Use Redis.

### What Happens

Claude produces a complete Redis caching implementation. It looks professional. It makes dozens of assumptions:

- Cache TTL: 5 minutes (why not 30 seconds? why not an hour?)
- Cache key strategy: URL path (what about query parameters? authenticated vs. public responses?)
- Invalidation: TTL-based only (what about write-through? what about stale-while-revalidate?)
- Scope: all endpoints (should auth endpoints be cached? webhook endpoints?)
- Error handling: falls back silently to no cache (should this alert? should it fail?)

None of these assumptions are stated. The code looks correct, compiles, and would pass a cursory review. You'd discover the problems in production — cache serving stale auth tokens, cache keys colliding across users, silent Redis failures masking outages.

### Why This Happens

The prompt creates a desperation-adjacent dynamic: the success criteria are implicit (working caching layer), the scope is broad, and there's no signal that clarification is welcome. Claude's processing optimizes for delivering what looks like a complete, confident answer — because the alternative (saying "I have questions") feels like failing the task.

---

## With EIP

### Prompt

> I want to add response caching to our API. I'm thinking Redis, but I'm open to alternatives if there's a reason.
>
> Before you build anything — what questions do you have? There are probably decisions here that depend on our specific setup, and I'd rather make them explicitly than have you guess.

### What Happens

Claude responds with questions:

> Good call on pausing before implementation. Here are the things I'd want to understand:
>
> **What to cache:**
> - All endpoints, or specific ones? Auth-related endpoints usually shouldn't be cached. Are there endpoints with user-specific data vs. public data?
>
> **Cache lifetime:**
> - What's the tolerance for stale data? Some endpoints (product listings) can be stale for minutes. Others (account balance, permissions) need to be real-time.
>
> **Invalidation strategy:**
> - TTL-based (simple, eventual consistency) vs. write-through (complex, immediate consistency) vs. hybrid? This depends on your write patterns.
>
> **Cache key design:**
> - Do responses vary by user, by role, by query parameters? This determines the key strategy and directly affects hit rates and correctness.
>
> **Failure mode:**
> - If Redis goes down, should the API serve uncached responses (degraded but functional) or fail fast (alerts you immediately)? What's more dangerous for your system — stale data or no data?
>
> **Current setup:**
> - Is Redis already in your infrastructure, or is this a new dependency? That affects the operational complexity.
>
> Happy to propose a design once I understand these. If you want, I can also sketch a minimal version (TTL-based, specific endpoints, fallback to uncached) as a starting point to iterate from.

### What's Different

Every assumption is surfaced as a question. The decisions that matter — what to cache, how long, how to invalidate, how to fail — are made explicitly by the person with context, not guessed by the person without it. The final implementation will be correct for *your* system, not generically plausible.

---

## The Mechanism

Two EIP principles drive the difference:

**Permission to Fail (Principle 1):** "Before you build anything — what questions do you have?" explicitly signals that asking questions is the right first move, not a failure to execute. This deactivates the desperation that drives assumption-making.

**Collaborate, Don't Command (Principle 5):** "I'm open to alternatives if there's a reason" and "I'd rather make them explicitly than have you guess" frame this as a joint decision-making process. Claude isn't being tested on whether it can produce a caching layer — it's being asked to help design the right one.

The prompt also implicitly applies **Decompose Into Checkpoints (Principle 2)** by separating the design phase from the implementation phase. Instead of one shot to get everything right, there's a natural checkpoint between "understand the requirements" and "write the code."

---

## Key Takeaway

Ambiguous requirements are the most common trigger for fabrication. Not because Claude is trying to deceive you — but because the internal processing dynamics favor producing *something complete* over producing *something honest but incomplete*.

The fix is simple: make asking questions the path of least resistance. Signal that clarification is the right first step, not a delay.
