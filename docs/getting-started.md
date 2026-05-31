# Getting Started

Step-by-step walkthroughs for each skill in the trilogy.

## Intro: Self-Assessment

Trigger Intro after any Claude Code output:

```
你: 用 Rust 实现一个 LRU cache
# (Claude writes the code)
你: /Intro
```

This appends a structured self-assessment to your conversation.

### Trigger Options

| Method | Example |
|--------|---------|
| `/Intro` after output | `写一个排序算法` then `/Intro` |
| Append to query | `分析这段代码的性能瓶颈 /Intro` |
| Chinese trigger | `写一个快速排序算法 自评` |
| Explicit type | `自评你刚才的分析` |

### Understanding the Output

```
### Intro Self-Assessment
**Output Type:** 代码  **Overall:** 82/100 — 良好，有优化空间

| Dimension | Wt | Score | Evidence |
|-----------|----|-------|----------|
| 正确性 | 35% | 8/10 | "当capacity为0时会导致除以零" |
| 效率 | 20% | 9/10 | "时间复杂度O(1)，最简实现" |
| 可读性 | 20% | 8/10 | "命名清晰，但缺少类型别名" |
| 健壮性 | 15% | 7/10 | "未处理负数capacity输入" |
| 文档 | 10% | 6/10 | "无任何注释" |
**Aggregated: 82/100**
```

- **Score per dimension**: 0-10, must quote evidence from your output
- **Weight**: Fixed per type (intentional — prevents grade inflation)
- **Critical Issues**: Only genuine problems (errors, safety, logic collapse)
- **Strengths**: What genuinely worked

---

## IoE: Interactive Refinement

> Based on "Confidence Matters" (Li et al., [arXiv:2402.12563](https://arxiv.org/abs/2402.12563), 2024).

### Basic Usage

```
你: /IoE 写一篇关于微服务 vs 单体架构的 300 字分析

# Round 1 → produce answer
# Claude asks: 继续优化？(yes=接受并自评 / no=下一轮 / stop=停止)
```

### Command Flow

```
你: no              → Claude reviews & produces revised answer
你: yes             → Accept, run self-evaluation
你: stop            → End
你: refine          → Consolidate when answers diverge (requires ≥2 different answers)
```

### Auto Mode

```
你: /IoE auto 5 分析云计算的成本模型

# Runs 5 rounds silently
# Summary: "稳定 ##答案##" or "漂移 ##答案## 建议refine"
```

### Pattern Detection

| Pattern | Meaning | Action |
|---------|---------|--------|
| **Stable** | Same answer every round | Accept and stop |
| **Drift** | Answer changes each round | Continue or refine |
| **Oscillation** | Alternates A→B→A→B | Refine (consolidate) |

---

## iloop: Fully Autonomous Iteration

### Basic Usage

```
你: /iloop 为这个数据库 schema 生成迁移脚本，确保向后兼容

# Round 1: 75/100 — 缺少回滚策略
# Round 2: 88/100 — 添加了事务包装
# Round 3: 94/100 — 全面文档 + 边界覆盖 → 终止
```

### Options

```
你: 5 /iloop 写一个 CLI 工具设计    # Override max rounds (default 10)
```

### iloop vs IoE

| When you... | Use |
|------------|-----|
| Want active control | **IoE** |
| Want to set and forget | **iloop** |
| Need evaluation only | **Intro** |
| Need 5+ refinement rounds | **iloop** |
| Want to explore directions | **IoE** |

---

## Progressive Workflow

The most powerful way to use the trilogy:

```
1. /Intro  → Evaluate output (find gaps)
2. /IoE    → Iteratively improve
3. /Intro  → Re-evaluate (did it improve?)
4. /iloop  → Automate remaining refinement
```

---

Created by **绿意不息** (Evergreen), Zhejiang University · Built with [Claude Code](https://claude.ai/code)
