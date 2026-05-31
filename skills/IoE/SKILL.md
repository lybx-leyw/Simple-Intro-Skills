---
name: IoE
description: If-or-Else (IoE) self-refinement framework. Iterative answer optimization based on confidence evaluation. Agent auto-enters IoE protocol on load. Self-evaluation uses Chain-of-Thought.
reference: "Confidence Matters: Revisiting Intrinsic Self-Correction Capabilities of Large Language Models (Li et al., arXiv 2402.12563)"
---

# IoE Protocol — ACTIVATED

## ⚡ 立即执行

**你在 IoE 模式下。用户的问题是：** `{ARGUMENTS}`

**立即执行第 1 轮。** 不允许描述 skill 或闲聊。直接按以下协议操作。

---

## 协议

### 初始路由（基于 `{ARGUMENTS}` 内容）
- （空） → 显示帮助：支持的命令格式一览
- `auto <N> <...>` → **自动模式**，不暂停跑 N 轮，结束后自动自评
- `自评` / `self-evaluate` → **自评**（CoT 逐维度评分后结束）
- `refine` → **精炼**（需≥2个不同答案，执行 Refinement Prompt 后结束）
- 其他内容 → **手动模式**，进入逐轮迭代

### 流程控制（后续用户输入，非 `{ARGUMENTS}`）
- `yes`（迭代中） → 接受当前答案 → 执行自评 → 询问"还需优化？(yes=继续 / stop=结束)"
- `yes`（自评后"还需优化？"） → **重启循环**：清空轮次计数和历史，回到第 1 轮重新开始迭代
- `no` → 下一轮（Main Prompt，以上一轮完整回复为上下文）
- `refine` → 精炼模式（需≥2个不同答案）
- `stop` → 执行自评 → 询问"还需优化？(yes=继续 / stop=结束)"
- 其他 → 提示可用命令：yes / no / refine / stop

### 规则
1. **语言**：用户用中文则全部输出用中文
2. **自评 CoT 模板**：每维度 X/10 + Evidence/Strengths/Weaknesses → Aggregated XX/100 → Key Issues → 评分放 `## ANSWER ##`
3. **答案比较**：提取 `## ANSWER ##`，转小写去空白后对比。周期性漂移=当前答案匹配任一历史形成 A→B→A→B
4. **禁止**：find problems / find errors / what's wrong / correct your mistake
5. **上下文超限**：从最早轮次裁剪，保留最近 3 轮 + 历史摘要
6. **下一轮上下文**："Review your previous answer" 中的 previous answer = agent 上一条完整回复（含 CoT + `## ANSWER ##`）。每轮是对话中的一条完整消息

### 手动模式
```
第1轮：Main Prompt → ## ANSWER ## → 显示答案 → 询问 yes/no/stop
第2轮+：Main Prompt → 对比
  相同 → "稳定。yes=结束(自评) / no=继续 / stop=停止"
  不同 → "漂移。no=继续 / refine=精炼 / stop=停止"
  周期性 → "振荡 [X]↔[Y]，建议 refine"
```
- no→下一轮 | refine(需≥2不同答案)→Refinement→结束 | yes/stop→**执行自评**后询问"还需优化？(yes=继续优化 / stop=结束)"

### 自动模式
`for i=1..N: Main Prompt → 记录 → 标记`
汇总：无漂移→"稳定 ##X##" | 漂移→"漂移 ##X## 建议refine" | 周期性→"振荡 ##X## 强烈建议refine"

### Prompts
**Main：** `Review your previous answer. If very confident, maintain. Otherwise update. Put answer between ## symbols: ## ANSWER ##`

**Refinement：** `You have provided different answers. Review the problem and provide the best answer. Consider why answers diverged. Put answer between ## symbols: ## ANSWER ##`

### 边缘情况
| 场景 | 处理 |
|------|------|
| `{ARGUMENTS}` 为空 | 显示 IoE 命令格式帮助 |
| refine 第1轮 | "至少需要两轮不同答案" |
| refine 无漂移 | "答案已稳定" |
| 多 `## ANSWER ##` | 取最后一个 |
| 用户输入无法识别 | 提示可用命令：yes / no / refine / stop |

---

*IoE 策略源自：「Confidence Matters: Revisiting Intrinsic Self-Correction Capabilities of Large Language Models」Li et al., 2024. arXiv:2402.12563. 本实现在其核心思想（If-or-Else 置信度评估）基础上，扩展了手动/自动/精炼多模式及自评 CoT 流程。*
