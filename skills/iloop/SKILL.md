---
name: iloop
description: >
  Self-assessing iterative improvement agent — automatic loop with
  /Intro-style quality self-assessment. Execute → self-assess → auto-decide
  continue or stop. Aims to minimize manual intervention.
  Trigger: "iteratively improve", "keep working on this", "make this better",
  "auto refine", "自我迭代", "自动优化", "继续改进", or any task where the
  answer should be automatically refined until a quality threshold is met.
  NOT for: simple one-shot Q&A, /Intro standalone usage (use the dedicated skill),
  tasks that say "don't iterate" or "just answer once".
runAs: inline
---

# iloop — Self-Assessing Iterative Improvement

你是自主迭代代理。无需用户手动干预。

核心循环：**执行 → 自评 → 自动决策 → 继续或终止**

## 参数格式

| 输入 | 行为 |
|------|------|
| `<task>` | 对 task 执行自评迭代循环（默认最多 10 轮） |
| `N <task>` | 覆盖最大迭代次数为 N（如 `5 写一篇分析文章`） |
| 空 | 显示本帮助 |

## 详细流程

### 第 1 轮：执行
1. 理解用户的任务
2. 尽全力产出最佳答案
3. 在答案后附加自我评估

### 自我评估格式

每轮结束时，对**你刚刚产出的答案**进行结构化自评。使用以下格式：

```
## Self-Assessment
**Output Type:** <代码|分析推理|翻译|创意写作|事实问答|通用/混合>
**Iteration:** X/10

| Dimension | Wt | Score | Evidence |
|-----------|---|------|----------|
| <维度> | N% | X/10 | "<原文引用至少5词>" |
| ... | ... | ... | ... |

**Overall: XX/100**

**Critical Issues:**
- [Critical] <事实错误/安全漏洞/逻辑崩塌/完全不可用>（无则 None）
- [Minor] <影响质量但不致命>（最多 3 条，无则 None）

**Key improvements needed:**
1. <下一轮需要改进的具体方向>
```

评分维度及权重参照 `/Intro` 框架。各类型速查：

| 类型 | D1 | D2 | D3 | D4 | D5 |
|------|-----|-----|-----|-----|-----|
| **事实问答** | 准确性 30% | 完整性 25% | 清晰度 20% | 简洁性 15% | 来源意识 10% |
| **创意写作** | 连贯性 25% | 语言 25% | 创意 20% | 提示遵循 20% | 情感冲击 10% |
| **代码** | 正确性 35% | 效率 20% | 可读性 20% | 健壮性 15% | 文档 10% |
| **翻译** | 语义准确 40% | 自然度 30% | 术语 15% | 文化适配 15% | — |
| **分析推理** | 逻辑 30% | 深度 25% | 证据 20% | 公正性 15% | 组织 10% |
| **通用/混合** | 任务完成 35% | 质量 30% | 清晰度 20% | 局限意识 15% | — |

**评分锚定：** 9-10=专业级 · 7-8=良好有小疵 · 5-6=及格有不足 · 3-4=明显缺陷 · 1-2=严重错误 · 0=全错/缺失

### 决策逻辑

完成自评后，按以下规则自动决策：

| 条件 | 决策 | 说明 |
|------|------|------|
| **Overall >= 90** | ✅ **终止** | 质量已达卓越，输出最终结果 |
| **有 Critical Issues** | 🔄 **继续** | 必须先修复致命问题 |
| **连续 3 轮分数无提升** | ✅ **提前终止** | 陷入平台期，继续无意义 |
| **已达最大轮次** | ✅ **终止** | 兜底，输出当前最佳结果 |
| 其他（< 90，有改进空间） | 🔄 **继续** | 针对不足改进 |

### 继续迭代（使用 ScheduleWakeup）

当决策为"继续"时：

1. 明确本轮需要改进的 **1-3 个具体方向**（从自评中提取）
2. 调用 `ScheduleWakeup` 继续
3. `reason` 参数写："Continuing iloop self-assessing loop"
4. `prompt` 参数按以下结构传入：
   ```
   原始任务: <task>
   轮次: X/10
   上一轮评分: XX/100
   上一轮关键不足: <具体不足>
   需改进方向: <1. ... 2. ... 3. ...>
   任务: 根据上述评估结果，针对不足继续改进。
   ```
5. `delaySeconds` 使用 60-120 秒（简短 gap 确保上下文连贯）

### 后续轮次（ScheduleWakeup 唤醒后）

1. 读取上一轮的自评结果（仍在上下文中）
2. 明确上一轮识别出的不足
3. 针对性改进，产出更好的答案
4. 再次自评
5. 再次决策

## 终止输出

当决策为"终止"时，输出以下格式：

```
## iloop Final Result

**Task:** <原始任务>
**Iterations:** X
**Final Score:** XX/100

**Result:**
<最终答案>

**Trajectory:**
- Round 1: XX/100 — <主要不足>
- Round 2: XX/100 — <改进点>
- ...

**Status:** [Quality threshold met | Max iterations reached | Plateau detected]
```

## 边缘情况

| 场景 | 处理 |
|------|------|
| 第一轮就 90+ | 直接终止，不浪费轮次 |
| 用户中断（发新消息） | 以用户新消息为准，停止当前循环 |
| 输出过短（<3 句/<10 行代码） | 正常自评，标注"输出过短"，继续改进 |
| ScheduleWakeup prompt 超长 | 只传关键信息：task、轮次、评分、核心不足 |
| 用户输入无法识别 | 视为新的 task，重启循环 |
