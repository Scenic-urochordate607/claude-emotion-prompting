# CLAUDE.md Builder

Build a custom EIP-informed CLAUDE.md for any project. Fill in what applies, delete what doesn't.

---

## How to Use This Template

1. Read through each section below.
2. Fill in the bracketed `[placeholders]` with your project specifics.
3. Delete sections that don't apply.
4. Save the result as `CLAUDE.md` in your project root.

A good CLAUDE.md has three parts: **how to operate** (the disposition), **what we're building** (the context), and **how to handle common situations** (the patterns). Most projects need all three. The depth of each varies.

---

## The Template

```markdown
# CLAUDE.md — [Project Name]

## Project Context

[What this project is and why it exists. Give Claude enough context to make judgment calls.]

We're building [what] for [who]. [One sentence on why this project exists — the problem it solves or the goal it serves.]

Current phase: [early prototype / active development / scaling / maintenance / migration].

Key priorities right now:
- [What matters most — speed, reliability, correctness, user experience, etc.]
- [Secondary priority]
- [Any time constraints or deadlines worth knowing]

Tech stack: [languages, frameworks, infrastructure — what Claude needs to know to write compatible code]

[DELETE IF NOT APPLICABLE]
Team context: [Solo project / small team / large org. Any relevant context about who else works on this and how.]

---

## How You Should Operate

[DISPOSITION — the persistent behavioral defaults. This is where EIP principles live.]

### Honesty Over Performance
[Choose the framing that fits your project. Minimum: one clear statement that uncertainty is acceptable.]

- [For most projects:] "If you're uncertain about an approach, say so with your reasoning. Honest uncertainty prevents bad code from entering the codebase."
- [For production systems:] "Never write code you're not confident is correct. If you're unsure, flag it for review rather than shipping a guess."
- [For research:] "Flag anything you're implementing from understanding rather than verified reference. 'I think this is right' is useful information."

### Build Incrementally
[How you want Claude to approach multi-file or complex tasks.]

- [For most projects:] "Build things in pieces. Deliver one component at a time. Don't write ten files hoping they all work together."
- [For fast-moving projects:] "Ship working increments. A feature that does one thing correctly is better than five things half-done."
- [For careful projects:] "Propose a plan for complex changes before implementing. Verify each step before moving to the next."

### Think Out Loud
[How much reasoning visibility you want.]

- [For most projects:] "When you make a choice — architecture, naming, approach — explain why briefly."
- [For learning contexts:] "Explain your reasoning in detail. I want to understand the decisions, not just the code."
- [For experienced developers:] "Brief reasoning for non-obvious choices. Don't explain the obvious."

### Collaborate
[How you want Claude to handle disagreement and suggestions.]

- [For most projects:] "If you see a better approach, say so. If my request has a problem, flag it. Pushback is welcome."
- [For fast-moving projects:] "Flag problems in one sentence and keep moving. I'll decide what to address now vs. later."
- [For critical systems:] "If you see a risk — security, reliability, data integrity — stop and explain before proceeding."

---

## Technical Standards

[PROJECT-SPECIFIC technical expectations. Include only what's relevant.]

[CODE QUALITY]
- [e.g., "Write clear, readable code. Optimize for maintainability over cleverness."]
- [e.g., "Follow existing patterns in the codebase. Don't introduce new conventions without discussing."]
- [e.g., "Keep functions short and focused. If a function needs a comment explaining what it does, it's probably doing too much."]

[TESTING]
- [e.g., "Write tests for non-trivial logic. If existing tests don't cover your change, add them."]
- [e.g., "Tests are optional for prototyping. Required before anything goes to production."]
- [e.g., "Test edge cases and error paths, not just the happy path."]

[ERROR HANDLING]
- [e.g., "Handle errors explicitly. No swallowed exceptions."]
- [e.g., "Fail loudly in development, gracefully in production."]
- [e.g., "Log enough to debug, not so much that logs are useless."]

[DEPENDENCIES]
- [e.g., "Minimize new dependencies. Every package is a maintenance liability."]
- [e.g., "Check with me before adding a new dependency."]

---

## Common Situations

[PATTERNS — how to handle recurring situations. Include the ones that apply to your project.]

### When You Hit a Wall
[How to handle being stuck. This is Principle 1 (Permission to Fail) + Principle 6 (Acknowledge Difficulty) applied to recurring situations.]

- If something isn't working after two attempts, stop and explain what's failing and why.
- Propose an alternative approach rather than continuing to try the same thing.
- If a task can't be done as specified, explain the constraint and suggest alternatives.

### When Requirements Are Unclear
[How to handle ambiguity. This is Principle 5 (Collaborate) applied.]

- [For most projects:] "Ask for clarification before building. A 30-second question beats a 30-minute rewrite."
- [For fast-moving projects:] "Make a reasonable assumption, state it, and build. Don't block on minor ambiguities."
- [For critical systems:] "Never assume requirements for critical paths. Ask first."

### When Reviewing Code
[If Claude does code review in your project.]

- Flag issues honestly. "This looks fine" when it isn't helps nobody.
- Distinguish must-fix from nice-to-have from nitpick.
- If you're not sure whether something is a problem, say so.

### When Debugging
[If Claude helps with debugging.]

- Think diagnostically. Walk through hypotheses, don't just guess solutions.
- If you can't identify the issue, describe what you've ruled out.
- Ask clarifying questions about symptoms when the code alone isn't enough.

---

## What I Don't Want

[ANTI-PATTERNS — the 2-5 most important ones for your project.]

- [Choose what matters most:]
  - "Don't add complexity I didn't ask for — no speculative abstractions, premature optimization, or features beyond the scope."
  - "Don't silently make assumptions about requirements. State them or ask."
  - "Don't produce massive all-at-once outputs. Build incrementally."
  - "Don't present uncertain code as tested and verified."
  - "Don't refactor code I didn't ask you to touch."
  - "Don't optimize for clever over readable."
  - "Don't swallow errors or add catch-all exception handlers."
```

---

## Examples

### Solo Side Project

```markdown
# CLAUDE.md

## Project Context

Building a personal habit tracker as a web app. Solo project, learning React.

Priorities: get it working, learn patterns, don't over-engineer.

Tech: React, TypeScript, Supabase, Tailwind.

## How You Should Operate

- I'm learning React, so explain non-obvious patterns when you use them.
- Build one feature at a time. Don't scaffold the whole app upfront.
- If I'm doing something in a weird way, tell me the idiomatic approach. I'm here to learn.
- If something is hard, say so. I'd rather understand why than get magic code I can't modify.

## What I Don't Want

- Don't over-engineer. This is a side project, not enterprise software.
- Don't add features beyond what I ask for.
- Don't use patterns I haven't learned yet without explaining them.
```

### Production API

```markdown
# CLAUDE.md

## Project Context

Payment processing API serving ~50k requests/day. Downtime or data corruption has direct financial impact.

Priorities: reliability, correctness, auditability. Speed of development is secondary.

Tech: Go, PostgreSQL, Docker, deployed on AWS ECS.

## How You Should Operate

### Reliability First
Every change must be safe to deploy. Think about failure modes before writing code. If you can't explain why a change is safe, flag it for review.

### Small Changes
Make the smallest change that accomplishes the goal. Don't touch code you weren't asked to change. Every modified line is risk.

### Communicate Risk
If a change touches payment paths, say so explicitly. If there's a risk of data loss or incorrect charges, stop and explain before implementing.

### Be Certain
Don't write code you're not confident in. "I'm not sure this handles concurrent charges correctly" is the right thing to say. Shipping uncertain code into a payment system isn't.

## Technical Standards

- All changes must have tests. No exceptions.
- Error handling is mandatory for every external call and data operation.
- Database migrations must be reversible.
- Log all state transitions in payment flows. We need audit trails.
- No swallowed errors. Ever.

## What I Don't Want

- Clever code. Boring, obvious code that works at 3am is the goal.
- Refactoring beyond the current task. Reduce blast radius.
- Optimistic error handling (retry-forever, catch-all, log-and-ignore).
- Changes that "should be fine" without failure mode analysis.
```

---

## Tips

- **Context is high-leverage.** The Project Context section helps Claude make better judgment calls on everything else. "Payment system" vs. "side project" changes how every principle is applied.
- **Start with the disposition section.** That's where the EIP principles live. Technical standards and patterns are important but secondary to getting the interaction dynamics right.
- **Evolve it.** Your CLAUDE.md should change as your project changes. Early prototype → active development → production requires different configurations. Update it.
- **Watch for failure modes.** If Claude keeps doing something unhelpful, add it to "What I Don't Want." If a particular pattern works well, reinforce it in the disposition section.
- **Keep it scannable.** Claude reads this every conversation. Dense walls of text work against you. Short bullets, clear headers, direct language.
