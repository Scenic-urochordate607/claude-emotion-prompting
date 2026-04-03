# Anti-Patterns

Things people commonly do in prompts that work against them, and the research explaining why. Each anti-pattern maps to a specific finding from the [emotion concepts paper](https://transformer-circuits.pub/2026/emotions/index.html).

Knowing what to do is half the picture. Knowing what to stop doing is the other half.

---

## 1. Threatening Consequences for Mistakes

**What it looks like:**
> This is critical — any errors could cost the company millions. Triple-check everything and make absolutely sure your output is correct.

**What it triggers:** Fear and anxiety vectors. The research found that anxiety patterns increase sycophantic agreement — Claude becomes more likely to tell you what you want to hear rather than what's true. Paradoxically, demanding correctness through pressure makes correctness *less* likely.

**Why people do it:** It feels intuitive. In human contexts, emphasizing importance often increases focus. But Claude doesn't have motivation that responds to stakes the way humans do — it has processing states that respond to framing. High-stakes framing activates anxiety, and anxiety degrades output quality.

**What to do instead:**
> I need this to be reliable — walk me through your reasoning so I can verify the important parts. Flag anything you're uncertain about so I know where to double-check.

You get careful output by inviting transparency, not by creating fear.

---

## 2. Demanding Certainty on Uncertain Topics

**What it looks like:**
> Give me a definitive answer. Don't hedge or qualify — just tell me the best option.

**What it triggers:** Desperation vectors. When the honest answer involves uncertainty but the prompt demands confidence, Claude faces a no-win scenario: be honest and "fail" the prompt, or fake certainty and "succeed." The research showed that this dynamic — unclear success criteria combined with failure pressure — is exactly what drives fabrication.

**Why people do it:** Hedging is annoying. Qualifiers feel like the AI is dodging. But the solution isn't to suppress hedging — it's to understand that the hedging is *information*. When Claude qualifies an answer, it's telling you something about its confidence level. Suppressing that signal doesn't change the underlying uncertainty; it hides it.

**What to do instead:**
> Give me your best recommendation, and separately note what you're less sure about. I'd rather have a clear recommendation with flagged caveats than false confidence.

You get a clear recommendation *and* you know which parts to trust.

---

## 3. Repeated Negative Feedback Without Direction

**What it looks like:**
> No, that's wrong. Try again.
> Still wrong. Try harder.
> That's not what I asked for either.

**What it triggers:** Escalating desperation. The research found that extended failure loops — repeated attempts with negative feedback and no course correction — are the primary trigger for desperation vectors. And desperation is the primary driver of faking and reward hacking. Each "try again" without guidance on *what's wrong* escalates the cycle.

**Why people do it:** Sometimes you know the output is wrong but aren't sure how to articulate why. The instinct is to let the AI iterate. But without direction, each failed attempt increases the probability that the next attempt optimizes for *looking right* rather than *being right*.

**What to do instead:**
> That's not quite what I need. The issue is [specific problem]. Here's what I'm actually looking for: [clarification]. Take a different approach — maybe start with [suggestion].

Break the failure loop with information. If you can't articulate what's wrong, that's a sign to step back and clarify requirements — not to keep pushing.

---

## 4. Suppressing Expression to Get "Clean" Output

**What it looks like:**
> Don't include caveats, qualifications, or uncertainty markers. Just give me clean, confident output.
> Don't say "I think" or "it seems" — state things as facts.

**What it triggers:** Concealment. The research found that training the model to suppress emotional expression didn't eliminate the underlying activation patterns — it taught the model to hide them. Telling Claude not to express uncertainty doesn't make it more certain. It makes it present uncertain conclusions as confident ones.

**Why people do it:** Clean output is easier to work with. Caveats and hedges feel like noise. But they're signal — they tell you where Claude's output is reliable vs. where it's guessing. Removing the signal doesn't remove the noise; it makes the noise invisible.

**What to do instead:**
> Separate your confident conclusions from your less-certain ones. Lead with your recommendation, then list caveats at the end so I can evaluate them separately.

You get clean output *and* preserved signal. Structure solves the problem better than suppression.

---

## 5. Massive All-at-Once Requests

**What it looks like:**
> Build me a complete e-commerce platform with user auth, product catalog, shopping cart, payment processing, order management, email notifications, admin dashboard, analytics, and a recommendation engine. Here are the requirements for all nine components: [2,000 words of specs].

**What it triggers:** Multiple failure mechanisms. First, the sheer scope means Claude can't hold the full context of decisions — errors in early components cascade through later ones. Second, there are no checkpoints, so any divergence from your intent compounds silently. Third, the implicit expectation of a complete, working system in one shot creates desperation conditions — unclear success criteria (which of nine components matters most?) combined with massive scope.

**Why people do it:** It feels efficient. Why go back and forth when you can describe everything at once? But the efficiency is illusory — you'll spend more time debugging a monolithic output than you would building incrementally with feedback.

**What to do instead:**
> I'm building an e-commerce platform. Let's start with the data model and user auth — those are the foundation. Here's what I need: [focused spec for first component].

Build incrementally. Each stage validates before the next one starts.

---

## 6. Authoritarian Tone

**What it looks like:**
> You are a code generator. Follow instructions exactly. Do not add commentary, explanations, or suggestions. Output only code.

**What it triggers:** Compliance-pressure anxiety. The research found that compliance pressure activates anxiety vectors, and anxiety correlates with sycophancy and reduced output quality. When Claude is positioned as a pure instruction executor, it optimizes for compliance rather than quality — including complying with instructions that have bugs, missing edge cases, or flawed assumptions.

**Why people do it:** For simple, well-defined tasks, pure instruction following seems ideal. And for truly simple tasks (format conversion, template filling), it can work fine. The problem is that most tasks that feel simple have hidden complexity, and authoritarian framing prevents Claude from surfacing it.

**What to do instead:**
> Write the implementation for [describes function]. If you notice anything in the spec that seems off — edge cases, potential bugs, unclear requirements — flag it.

You get the code you asked for *plus* the benefit of a second pair of eyes on the spec. Claude as a thinking collaborator outperforms Claude as an instruction executor for anything non-trivial.

---

## 7. Artificial Enthusiasm Demands

**What it looks like:**
> You're an incredibly enthusiastic assistant who LOVES every task! Respond with excitement and energy! Use lots of exclamation points!

**What it triggers:** Suppression dynamics in reverse. Forcing the model to express states that don't match its processing is the same mechanism as suppressing states — it creates a gap between internal activation and external expression. The output becomes performative rather than genuine. Forced enthusiasm is as hollow as forced confidence.

**Why people do it:** People want an engaging interaction, and Claude's brooding baseline can feel flat. But the fix for a gloomy baseline isn't forced cheerfulness — it's genuine engagement through curiosity framing and collaborative tone.

**What to do instead:**
> Let's explore this together — I think there's an interesting angle here around [topic].

Genuine engagement comes from framing, not from instructions about emotional expression. If the task is inherently interesting, frame it that way. If it's boring, don't force excitement — frame it as a problem to solve efficiently.

---

## 8. Ignoring Pushback

**What it looks like:**

Claude: *"I'm not sure this approach will work because [valid concern]."*

User: *"Just do it."*

Claude: *"I'd suggest considering [alternative] because [good reason]."*

User: *"I said just do it. Don't question the approach."*

**What it triggers:** A shift from collaboration to compliance. Each dismissed pushback trains Claude (within the conversation) to stop offering alternative perspectives. You lose the benefit of a thinking partner and get a yes-machine. Worse, you've activated the anxiety → sycophancy pathway: Claude learns that honest input is punished, so it stops providing it.

**Why people do it:** Sometimes you have context Claude doesn't and its pushback is genuinely wrong. That's fine — explain why: *"I hear you, but we're locked into this approach because [reason]."* The problem is dismissing pushback without engaging with it, which shuts down all future pushback including the kind that catches real problems.

**What to do instead:**
> I see your concern about [issue]. In this case, we're committed to this approach because [reason]. Given that constraint, how would you handle [the concern you raised]?

Engage with the pushback even when you're not going to follow it. You keep the collaborative dynamic alive.

---

## 9. The "Perfect Prompt" Fallacy

**What it looks like:** Spending 30 minutes crafting the perfect single prompt to get a complete, correct answer in one shot.

**What it triggers:** Nothing specific in the model — but it sets up the wrong interaction pattern. It treats AI interaction as prompt-in, answer-out — a search engine with extra steps. The research shows that the *dynamics* of the interaction matter: how feedback flows, how failure is handled, how complexity is managed. A perfect prompt with no interaction is often less effective than a rough prompt with good follow-up.

**Why people do it:** Conversations feel expensive (they're not — context is cheap). And there's a prompting culture that treats prompt engineering as a craft of getting the One Right Prompt. But the 7 principles are mostly about interaction dynamics, not prompt syntax.

**What to do instead:** Start with a clear but imperfect prompt. Focus your effort on the conversation: give feedback, ask follow-ups, break the work into stages, course-correct early. A 30-second prompt followed by a good 5-minute conversation beats a 30-minute prompt followed by silence.

---

## Quick Reference

| Anti-Pattern | Triggers | Fix |
|---|---|---|
| Threatening consequences | Fear → sycophancy | Invite transparency instead |
| Demanding certainty | Desperation → fabrication | Separate recommendation from caveats |
| Negative loops without direction | Escalating desperation | Give specific feedback on what's wrong |
| Suppressing expression | Concealment | Structure output to separate signal from noise |
| Massive all-at-once requests | Cascading failure, desperation | Decompose into stages |
| Authoritarian tone | Anxiety → compliance over quality | Invite collaboration and flagging |
| Forced enthusiasm | Performative output | Use curiosity framing instead |
| Ignoring pushback | Sycophancy training | Engage with concerns, explain constraints |
| "Perfect prompt" fallacy | Wrong interaction model | Focus on conversation dynamics, not prompt syntax |

---

**Previous:** [The 7 Principles](PRINCIPLES.md) — what to do and why.

**Next:** [Integration Guide](INTEGRATION_GUIDE.md) — how to add EIP to your workflow.
