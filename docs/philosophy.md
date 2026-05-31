# Design Philosophy

The thinking behind the Quality Trilogy — why these skills exist, what problems they solve, and the design decisions that shaped them.

## The Problem: LLM Output Is a Distribution

The same model, same prompt, same temperature — two different answers. **When you get one answer, you don't know where it falls in the distribution.** Is it the best possible? The worst? Somewhere in the middle?

Traditional approaches:
- **Human review** — doesn't scale, humans are inconsistent
- **Multiple samples + selection** — picks from the distribution but doesn't improve any individual sample
- **Single-shot with "be careful"** — aspirational, not reliably effective

The trilogy offers a different approach.

## The Insight: Self-Evaluation Is the Lever

A language model can evaluate its own output. Not perfectly — but well enough to identify specific weaknesses, compare two answers, and improve based on identified gaps.

> **The model has more information about quality at evaluation time than at generation time**, because it has the actual output to analyze.

## The Three Skills as a Progression

### Intro: The Measurement Foundation

**You cannot improve what you cannot measure.** Intro is deliberately:
- **Rigorous** — evidence-based, weighted dimensions, no grade inflation
- **Limited** — evaluates only. No revision, no rewriting.
- **Honest** — calibrated so 60 means 60, not "I don't want to be mean"

Without Intro, "improve this" is vague. With Intro: "Completeness is 6/10 because the analysis missed the trade-off section."

### IoE: The Comparison Protocol

Adapted from *Confidence Matters* (Li et al., [arXiv:2402.12563](https://arxiv.org/abs/2402.12563), 2024). The original paper showed that LLMs can assess their own confidence, and an "If-or-Else" prompt (maintain if confident, revise if uncertain) improves accuracy across benchmarks.

IoE introduces **comparison as a signal**. Three patterns emerge:

| Pattern | Meaning | Action |
|---------|---------|--------|
| **Stable** | Answers consistent. Model is confident. | Accept |
| **Drift** | Answers differ. Model is uncertain. | Iterate or refine |
| **Oscillation** | Alternates A→B→A→B. Model is conflicted. | Refine (consolidate) |

The "禁止" list (`find problems`, `what's wrong`) is deliberate: these trigger defensive/corrective modes that undermine honest comparison.

### iloop: Full Automation

If the model can evaluate and improve itself, why does a human need to be in the loop? **For certain tasks, they don't.**

iloop combines Intro's scoring with automated iteration. The 10-round default and plateau detection (no improvement for 3 rounds) prevent infinite loops. The system isn't trying to iterate forever — it's reaching a quality target efficiently.

## Key Design Decisions

### 1. SKILL.md as the Atomic Unit

Each skill is one self-contained file. No dependencies, no build step. Drop it in `~/.claude/skills/` and it works:
- **Self-contained** — everything the agent needs is in one file
- **Portable** — works across machines, syncs via dotfiles
- **Immutable** — the SKILL.md is the source of truth

### 2. Fixed Weights (by Default)

The weights encode a value judgment. Code correctness (35%) arguably matters more than code documentation (10%). Factual accuracy (30%) perhaps more than conciseness (15%). Allowing weights to be freely changed could let users tailor the system for flattering scores, so we keep them fixed — but feel free to adjust if your use case calls for it.

### 3. Edge Cases Are Documented

Every skill has an edge case table. The most common failure mode for AI agent prompts is unhandled edge cases — empty input, context overflow, unclear type, user interruption. These aren't afterthoughts; they're part of the spec.

## Limitations

- **Not a benchmark** — evaluates a specific output, not general model capability
- **Not a replacement for human review** — especially for high-stakes content
- **Not a fine-tuning pipeline** — improvements don't persist across sessions
- **Not a silver bullet** — some tasks are genuinely hard; no amount of self-iteration fixes that

## Future Directions

Possible extensions: introspection logger, multi-model IoE, quality dashboard, skill composition.

---

Created by **绿意不息** (Evergreen), Zhejiang University · Built with [Claude Code](https://claude.ai/code)
