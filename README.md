# Intro · IoE · iloop — The Quality Trilogy

**Measure.** **Compare.** **Iterate.** **Automate.**

Three [Claude Code](https://claude.ai/code) skills that form a progressive quality system for AI-generated content: evidence-based self-assessment, iterative refinement through answer comparison, and fully autonomous improvement loops.

[![Skill: Intro](https://img.shields.io/badge/Skill-Intro-6A4C93)](skills/Intro/SKILL.md)
[![Skill: IoE](https://img.shields.io/badge/Skill-IoE-1982C4)](skills/IoE/SKILL.md)
[![Skill: iloop](https://img.shields.io/badge/Skill-iloop-00A878)](skills/iloop/SKILL.md)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen)](CONTRIBUTING.md)

## Overview

Each skill builds on the one before it. Use them independently or as a progression:

| # | Skill | What It Does | Your Role |
|---|-------|-------------|-----------|
| 1 | **Intro** | Scores any output across 6 types with evidence-based, weighted dimensions | **Evaluate** — "How good is this?" |
| 2 | **IoE** (If-or-Else) | Iteratively refines answers by detecting stability, drift, and oscillation patterns | **Guide** — "Keep going / stop / refine" |
| 3 | **iloop** | Fully autonomous execute → self-assess → decide → continue/terminate loop | **Walk away** — "Make this great" |

## Quick Start

### Installation

```bash
# Copy all skills to Claude Code
git clone <repo-url>
cp -r skills/* ~/.claude/skills/
```

**Windows (PowerShell):**
```powershell
Copy-Item -Recurse skills\* $env:USERPROFILE\.claude\skills\
```

**Or link individual skills (macOS/Linux):**
```bash
ln -s $(pwd)/skills/Intro ~/.claude/skills/Intro
ln -s $(pwd)/skills/IoE ~/.claude/skills/IoE
ln -s $(pwd)/skills/iloop ~/.claude/skills/iloop
```

### Verify

```
/Intro    →  scoring framework and usage guide
/IoE      →  activate IoE protocol
/iloop    →  iloop usage guide
```

## Quick Examples

**Intro — Self-assessment of any output:**
```
你: 写一个 Python 函数检查回文
你: /Intro
    Correctness 9/10, Efficiency 8/10, Readability 9/10 → 87/100
```

**IoE — Interactive refinement:**
```
你: /IoE 分析 Transformer 和 Mamba 架构的 trade-offs
    Round 1 → answer A, Round 2 → answer B (drift detected)
你: refine → synthesized answer C
你: yes → self-evaluation
```

**iloop — Fully automated iteration:**
```
你: /iloop 写一份生产就绪的错误处理中间件
    Round 1: 72/100 → Round 2: 84/100 → Round 3: 93/100 ✓
    Final result with full trajectory delivered
```

## How They Work Together

```
┌─────────────────────┐
│       Intro         │  measurement standard
│  (scoring engine)   │
└────────┬────────────┘
         │ provides dimensions & anchors
         ▼
┌─────────────────────┐
│        IoE          │  guided improvement
│  (comparison loop)  │
└────────┬────────────┘
         │ automates the loop
         ▼
┌─────────────────────┐
│       iloop         │  autonomous iteration
│  (self-improvement) │
└─────────────────────┘
```

## Resources

| What | Where |
|------|-------|
| Getting started guide | [docs/getting-started.md](docs/getting-started.md) |
| Scoring dimensions & weights | [docs/quality-framework.md](docs/quality-framework.md) |
| Design rationale | [docs/philosophy.md](docs/philosophy.md) |
| Intro examples | [examples/intro-example.md](examples/intro-example.md) |
| IoE examples | [examples/ioe-example.md](examples/ioe-example.md) |
| iloop examples | [examples/iloop-example.md](examples/iloop-example.md) |
| Intro skill | [skills/Intro/SKILL.md](skills/Intro/SKILL.md) |
| IoE skill | [skills/IoE/SKILL.md](skills/IoE/SKILL.md) |
| iloop skill | [skills/iloop/SKILL.md](skills/iloop/SKILL.md) |

## Academic Reference

The IoE skill is based on:

> **Confidence Matters: Revisiting Intrinsic Self-Correction Capabilities of Large Language Models**  
> Li et al., arXiv:2402.12563, 2024 · [https://arxiv.org/abs/2402.12563](https://arxiv.org/abs/2402.12563)

This implementation extends the original "If-or-Else" confidence evaluation strategy with multi-mode interaction and CoT self-assessment.

## About

Created by **绿意不息** (Evergreen), Zhejiang University, with [Claude Code](https://claude.ai/code). These skills grew from a simple observation: LLM outputs vary, and structured self-evaluation is the most reliable way to control that variance.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). Issues and PRs welcome.

## License

MIT © 2026 绿意不息 (Zhejiang University)
