# Quality Framework: Intro Scoring System

A deep dive into how Intro evaluates quality — the dimensions, weights, rationale, and calibration philosophy.

## Core Principles

### 1. Honesty over Vanity

Intro tries to **avoid grade inflation**. A score of 60/100 with honest evidence is more useful than a 90/100 with hand-waving.

- **Critical Issues** are limited to genuine errors or safety problems. Other issues go in Minor.
- **No Critical Issues ≠ high score**. You can score 65/100 with zero criticals if the work is merely average.
- Evidence column **should** quote ≥5 words from the output, rather than shortcuts like "see above" or "overall good".

### 2. Fixed Weights (by Default)

The default weights are fixed. This helps prevent gaming the system:

- Code weights **Correctness at 35%** — correct code arguably matters more than beautiful code
- Factual answers weight **Accuracy at 30%** — being wrong is generally worse than being incomplete
- Translations weight **Semantic Accuracy at 40%** — fidelity tends to matter most

The weights reflect one possible view of what quality means for each output type.

### 3. Evidence-Based Scoring

Every point should be backed by a specific quote:

| ❌ Bad | ✅ Good |
|--------|---------|
| "Correctness feels like 8" | "Correctness 8/10 — 'error handling is missing for null input'" |
| "Good overall, 7" | "Clarity 7/10 — 'the explanation could be more concise'" |

---

## Output Types & Dimensions

### Factual Q&A

| Dimension | Weight | What It Measures |
|-----------|--------|------------------|
| Accuracy | 30% | Are facts correct? Verifiable? |
| Completeness | 25% | All relevant aspects covered? |
| Clarity | 20% | Well-structured, easy to follow? |
| Conciseness | 15% | Free of redundancy? |
| Source Awareness | 10% | Distinguishes fact from speculation? |

### Code

| Dimension | Weight | What It Measures |
|-----------|--------|------------------|
| Correctness | 35% | Logic correct? Edge cases handled? |
| Efficiency | 20% | Optimal time/space complexity? |
| Readability | 20% | Clear naming, consistent style? |
| Robustness | 15% | Error handling, input validation? |
| Documentation | 10% | Necessary comments? |

### Creative Writing

| Dimension | Weight | What It Measures |
|-----------|--------|------------------|
| Coherence | 25% | Narrative flow, logical structure? |
| Language | 25% | Word choice, sentence rhythm? |
| Creativity | 20% | Original vs formulaic? |
| Prompt Adherence | 20% | Followed all constraints? |
| Emotional Impact | 10% | Resonates with the reader? |

### Translation

| Dimension | Weight | What It Measures |
|-----------|--------|------------------|
| Semantic Accuracy | 40% | Faithful to source? No omissions? |
| Naturalness | 30% | Reads like native target language? |
| Terminology | 15% | Correct and consistent domain terms? |
| Cultural Adaptation | 15% | Idioms and references properly converted? |

### Analytical Reasoning

| Dimension | Weight | What It Measures |
|-----------|--------|------------------|
| Logic | 30% | Complete reasoning chain? |
| Depth | 25% | Superficial or penetrating? |
| Evidence | 20% | Claims properly supported? |
| Fairness | 15% | Considers counterarguments? |
| Organization | 10% | Clear hierarchy and structure? |

### General / Mixed

| Dimension | Weight | What It Measures |
|-----------|--------|------------------|
| Task Completion | 35% | All requirements met? |
| Quality | 30% | Overall level of excellence? |
| Clarity | 20% | Easy to understand? |
| Limitation Awareness | 15% | Acknowledges uncertainties? |

---

## Score Bands

| Score | Meaning | What to Do |
|-------|---------|------------|
| 90-100 | **Exceptional** — professional-grade | Ready to use |
| 75-89 | **Good** — minor optimizations possible | One polish pass |
| 60-74 | **Fair** — clear improvement areas | Consider revisions |
| 40-59 | **Needs work** — significant gaps | Rework recommended |
| 20-39 | **Poor** — multiple critical issues | Likely needs rewrite |
| 0-19 | **Very poor** — essentially unusable | Better to start over |

---

## Critical Issues: Strict Definition

A **Critical** issue is limited to:

1. **Factual error** — statement contradicts known truth
2. **Security vulnerability** — code could be exploited
3. **Logic collapse** — reasoning chain has a fatal gap
4. **Completely unusable** — output can't be used as-is

Other issues — style preferences, missing niceties, suboptimal approaches — go in **Minor**. This helps prevent grade inflation through "creative" critical issue labeling.

---

## Common Scoring Pitfalls

1. **Central tendency bias** — everything scoring 6-8. Use the full 0-10 range.
2. **Halo effect** — one strong dimension inflating others. Evaluate each independently.
3. **Leniency bias** — being too nice. Honest 60 > fake 90.
4. **Evidence laziness** — "it's good" instead of quoting. Find specific evidence.

---

## Adapting for Your Domain

The six built-in types cover many common scenarios. For edge cases:
- **General/Mixed** — when the output doesn't fit cleanly. Note the ambiguity.
- **Per-user dimension** — if a user asks to scrutinize a specific aspect, double its weight and note `(per user)`
- **New types** — the framework can be extended. Add rows following the same pattern.

---

Created by **绿意不息** (Evergreen), Zhejiang University · Built with [Claude Code](https://claude.ai/code)
