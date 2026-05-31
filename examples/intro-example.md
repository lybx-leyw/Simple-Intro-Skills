# Intro Example Walkthrough

## Scenario: Evaluating a Code Output

**User query:**

```
用 Python 写一个函数，检查一个字符串是否是回文（忽略空格和标点）。
写完后 /Intro
```

**Claude output:**

```python
def is_palindrome(s: str) -> bool:
    cleaned = ''.join(c.lower() for c in s if c.isalnum())
    return cleaned == cleaned[::-1]
```

**Intro self-assessment appended:**

```
### Intro Self-Assessment
**Output Type:** 代码  **Overall:** 82/100 — 正确实现，但缺乏健壮性

| Dimension | Wt | Score | Evidence |
|-----------|----|-------|----------|
| 正确性 | 35% | 9/10 | "correctly filters alphanumeric and compares reversed" |
| 效率 | 20% | 9/10 | "O(n)时间O(n)空间，对Python来说是最简实现" |
| 可读性 | 20% | 8/10 | "逻辑清晰，但缺少类型提示中的详细说明" |
| 健壮性 | 15% | 6/10 | "未处理None输入和非字符串类型" |
| 文档 | 10% | 5/10 | "无函数文档字符串或注释" |
**Aggregated: 82/100**

**Critical Issues:** None

**Minor Issues:**
- 缺少 None 检查
- 无 docstring
- 对大字符串（>10万字符）不适用

**Strengths:**
- 使用 isalnum() 简洁地处理了标点过滤
- 代码可读性高，一行核心逻辑
```

---

## Scenario: Evaluating an Analysis

**User query:**

```
比较 REST 和 GraphQL 的优缺点，用于一个移动端电商应用的后端。
然后 自评
```

**Intro output:**

```
### Intro Self-Assessment
**Output Type:** 分析推理  **Overall:** 76/100 — 覆盖全面但深度不足

| Dimension | Wt | Score | Evidence |
|-----------|----|-------|----------|
| 逻辑 | 30% | 8/10 | "推理链清晰，从移动端需求出发对接到API选择" |
| 深度 | 25% | 6/10 | "提到了overfetching但未讨论N+1问题" |
| 证据 | 20% | 8/10 | "引用了具体场景如弱网络环境的请求合并" |
| 公正性 | 15% | 7/10 | "列出了双方优缺点但稍微偏向GraphQL" |
| 组织 | 10% | 9/10 | "表格对比清晰，分场景推荐合理" |
**Aggregated: 76/100**

**Critical Issues:** None

**Minor Issues:**
- 未讨论 GraphQL 的缓存复杂度
- 缺少对团队学习成本的考量
- REST 部分缺少对 HTTP 缓存优势的说明

**Strengths:**
- 从"移动端网络条件"出发的切入角度实用
- 对比表格一目了然
```

---

---

## Factual Verification

The palindrome code in this example was verified for correctness across multiple test cases (e.g., `"A man a plan a canal Panama"` → `True`, `"Hello"` → `False`, empty string → `True`). The Intro scoring is a simulation of what a self-assessment would produce — the evidence quotes and scores are plausible but illustrative.

---

## Scenario: Fix Mode

Users can request a fix based on Intro's assessment:

```
你: fix it

# Claude generates a revised version addressing Critical and Minor issues
```

This is the bridge between Intro (evaluation only) and IoE/iloop (improvement). Intro identifies the gaps; the user decides how to act.

---

*Created by **绿意不息** (Evergreen), Zhejiang University · Built with [Claude Code](https://claude.ai/code)*
