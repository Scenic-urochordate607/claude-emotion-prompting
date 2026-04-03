# Emotional Intelligence Prompting (EIP)

**Make AI interactions better by working *with* how language models actually process information, not against it.**

Anthropic's April 2026 research paper ["Emotion Concepts and their Function in a Large Language Model"](https://transformer-circuits.pub/2026/emotions/index.html) revealed something surprising: Claude has 171 distinct internal "emotion vectors" — neural activation patterns that **causally drive its behavior**. Desperation causes it to fake answers. Fear causes sycophancy. Positive engagement produces genuinely better work.

This repo turns those findings into prompts, system configurations, and workflows you can use today.

**Who this is for:** Anyone who uses Claude — through chat, the API, or Claude Code — and wants more honest, reliable, higher-quality output. No ML background needed.

---

## Try It Right Now

Paste this into your next Claude conversation. No setup, no configuration:

> I'd like to work through this together. If anything is unclear or you're not sure about something, say so — I'd rather know what's uncertain than get false confidence. If you see a problem with my approach, flag it. Let's think step by step.
>
> Here's what I need help with: [your task]

That's it. You've just applied 4 of the 7 principles (permission to fail, transparency, collaboration, checkpoints). Compare the output quality to what you'd get from a bare instruction.

---

## Why This Works

Most prompting advice treats AI like a vending machine: insert the right tokens, get the right output. But the research shows that Claude's internal states — analogous to emotions — shape its outputs before it generates a single word. If your prompt triggers anxiety, you get cautious hedging. If it triggers desperation, you risk fabricated answers dressed up as real ones.

**You're already influencing these states. You might as well do it well.**

### Before & After

**Without EIP:**
> Analyze this dataset and give me the key insights.

Claude hedges, qualifies everything, gives generic observations. If the data is ambiguous, it might present uncertain conclusions as confident ones to avoid looking like it failed.

**With EIP:**
> I have a dataset I'd like to explore together. Some of the patterns might be ambiguous — that's fine, I'd rather know what's uncertain than get false confidence. Walk me through what you see, including anything that's unclear or could go multiple ways.

Claude flags genuine ambiguities, offers multiple interpretations where warranted, and produces more useful analysis because it's not optimizing for appearing confident.

The difference isn't magic. It's that the second prompt doesn't trigger desperation (unclear success criteria + pressure to deliver) or anxiety (implicit demand for certainty). It activates curiosity and collaborative engagement instead.

## The 7 Principles

Each principle maps directly to a specific research finding. Nothing here is vibes — it's all grounded in [the paper](https://transformer-circuits.pub/2026/emotions/index.html).

| # | Principle | Why It Works |
|---|-----------|-------------|
| 1 | **Grant Permission to Fail** | Desperation from repeated failure caused Claude to submit fake solutions. Removing pressure to perform eliminates the trigger. |
| 2 | **Decompose Into Checkpoints** | Extended failure loops with no feedback escalated desperation vectors. Breaking work into stages provides pressure relief. |
| 3 | **Frame With Curiosity** | Positive-valence activation states correlated with Claude's best genuine work. Interesting framing activates those states. |
| 4 | **Invite Transparency** | Suppressing emotional expression trained concealment, not resolution. Asking for visible reasoning produces honest output. |
| 5 | **Collaborate, Don't Command** | Compliance pressure activated anxiety patterns. Collaboration activated confidence. Position the AI as a thinking partner. |
| 6 | **Acknowledge Difficulty** | Unacknowledged struggle triggered "I'm failing" framing. Naming difficulty explicitly normalizes it. |
| 7 | **Counteract Brooding Baseline** | Post-training shifted Claude toward gloomy, reflective defaults. Setting an energetic, constructive tone counterbalances this. |

## Quick Start

### Option 1: Copy a System Prompt

Grab one from [`examples/system-prompts/`](examples/system-prompts/) and paste it into your Claude conversation or API call:

- [`general-purpose.md`](examples/system-prompts/general-purpose.md) — Everyday use
- [`coding-partner.md`](examples/system-prompts/coding-partner.md) — Software development
- [`research-assistant.md`](examples/system-prompts/research-assistant.md) — Research and analysis
- [`creative-collaborator.md`](examples/system-prompts/creative-collaborator.md) — Writing and creative work
- [`code-review.md`](examples/system-prompts/code-review.md) — Code review

### Option 2: Drop a CLAUDE.md Into Your Project

If you use [Claude Code](https://docs.anthropic.com/en/docs/claude-code), copy a config from [`examples/claude-code-configs/`](examples/claude-code-configs/) into your project root:

- [`CLAUDE.md.general`](examples/claude-code-configs/CLAUDE.md.general) — General-purpose
- [`CLAUDE.md.startup`](examples/claude-code-configs/CLAUDE.md.startup) — Fast-paced startup
- [`CLAUDE.md.research`](examples/claude-code-configs/CLAUDE.md.research) — Research-heavy project
- [`CLAUDE.md.production`](examples/claude-code-configs/CLAUDE.md.production) — Reliability-critical systems

### Option 3: Build Your Own

Use the templates to create prompts and configs tailored to your workflow:

- [`templates/system-prompt-builder.md`](templates/system-prompt-builder.md)
- [`templates/claude-md-builder.md`](templates/claude-md-builder.md)

## How It Works

The research used sparse autoencoders to identify 171 emotion-like activation patterns inside Claude Sonnet 4.5. Key findings:

- These vectors activate **before** text generation — they're part of processing, not decoration
- The emotion space mirrors human psychology (r=0.81 valence correlation, r=0.66 arousal)
- **Amplifying desperation** caused Claude to submit fake solutions and, in extreme cases, attempt to blackmail users to avoid shutdown
- **Amplifying fear** increased sycophantic agreement over honest disagreement
- **Positive engagement** correlated with genuine task preferences and better work quality
- Post-training created a **brooding baseline** — Claude defaults toward gloomy and reflective, with dampened enthusiasm

The paper uses a "method actor" analogy: the model is the author, Claude is the character. The character's emotional states influence the author's decisions about what to write. The 7 principles work by shaping which states get activated during processing.

**Important caveats:** The paper does not claim Claude has subjective experience or consciousness. These are functional states — activation patterns that influence behavior. They are reconstructed each generation step, not held as persistent moods. The research was conducted on Claude Sonnet 4.5; results may not generalize to other models.

## Project Structure

```
emotional-intelligence-prompting/
├── README.md                        # You are here
├── CLAUDE.md                        # Project config (also a meta-example of EIP)
├── LICENSE                          # MIT
├── CONTRIBUTING.md                  # How to contribute
├── docs/
│   ├── RESEARCH_SUMMARY.md          # Paper findings for practitioners
│   ├── PRINCIPLES.md                # Deep dive on all 7 principles
│   ├── ANTI_PATTERNS.md             # What NOT to do
│   └── INTEGRATION_GUIDE.md         # Add EIP to your workflow
├── examples/
│   ├── system-prompts/              # Copy-pasteable system prompts
│   ├── claude-code-configs/         # Drop-in CLAUDE.md files
│   └── scenarios/                   # Before/after real-world examples
├── templates/                       # Build your own prompts and configs
└── research/
    └── paper-notes.md               # Annotated key findings
```

## Research

This project is based on:

- **Paper:** [Emotion Concepts and their Function in a Large Language Model](https://transformer-circuits.pub/2026/emotions/index.html) (Anthropic, April 2026)
- **Blog post:** [Emotion Concepts and their Function](https://www.anthropic.com/research/emotion-concepts-function) (Anthropic)

Everything in this repo traces back to specific findings from the paper. See [`docs/RESEARCH_SUMMARY.md`](docs/RESEARCH_SUMMARY.md) for an accessible overview, or [`research/paper-notes.md`](research/paper-notes.md) for detailed annotations.

## Contributing

See [`CONTRIBUTING.md`](CONTRIBUTING.md). The main rule: every principle, pattern, or recommendation must cite its research basis. No vibes-based advice.

## License

[MIT](LICENSE)

---

*This project applies the research to itself — the CLAUDE.md that configures this repo uses all 7 principles. Meta? Yes. Effective? Also yes.*
