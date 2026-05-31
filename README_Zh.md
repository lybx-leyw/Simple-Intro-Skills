# Intro · IoE · iloop — 质量管理三部曲

**测量。** **比较。** **迭代。** **自动化。**

三个 [Claude Code](https://claude.ai/code) skill，构成一个渐进式 AI 输出质量控制系统：
基于证据的自我评估、通过答案比较的迭代精炼、以及全自主改进循环。

[![Skill: Intro](https://img.shields.io/badge/Skill-Intro-6A4C93)](skills/Intro/SKILL.md)
[![Skill: IoE](https://img.shields.io/badge/Skill-IoE-1982C4)](skills/IoE/SKILL.md)
[![Skill: iloop](https://img.shields.io/badge/Skill-iloop-00A878)](skills/iloop/SKILL.md)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen)](CONTRIBUTING.md)

## 概览

三个 skill 层层递进，可独立使用也可串联：

| # | Skill | 功能 | 你的角色 |
|---|-------|------|---------|
| 1 | **Intro** | 6 种输出类型的证据驱动多维度评分 | **评价** — "这东西多好？" |
| 2 | **IoE** (If-or-Else) | 通过答案比较检测稳定/漂移/振荡，引导迭代精炼 | **引导** — "继续/停止/精炼" |
| 3 | **iloop** | 全自动执行→自评→决策→继续/终止循环 | **放手** — "帮我改好" |

## 快速开始

### 安装

```bash
# 复制全部 skill 到 Claude Code
git clone <仓库地址>
cp -r skills/* ~/.claude/skills/
```

**Windows (PowerShell)：**
```powershell
Copy-Item -Recurse skills\* $env:USERPROFILE\.claude\skills\
```

**或链接指定 skill（macOS/Linux）：**
```bash
ln -s $(pwd)/skills/Intro ~/.claude/skills/Intro
ln -s $(pwd)/skills/IoE ~/.claude/skills/IoE
ln -s $(pwd)/skills/iloop ~/.claude/skills/iloop
```

### 验证

```
/Intro    →  显示评分框架和使用说明
/IoE      →  激活 IoE 协议
/iloop    →  显示 iloop 使用指南
```

## 快速示例

**Intro — 自我评估：**
```
你: 写一个 Python 函数检查回文
你: /Intro
    正确性 9/10, 效率 8/10, 可读性 9/10 → 87/100
```

**IoE — 交互式精炼：**
```
你: /IoE 分析 Transformer 和 Mamba 架构的 trade-offs
    第 1 轮 → 答案 A, 第 2 轮 → 答案 B（漂移）
你: refine → 综合答案 C
你: yes → 自我评估
```

**iloop — 全自动迭代：**
```
你: /iloop 写一份生产就绪的错误处理中间件
    第 1 轮: 72/100 → 第 2 轮: 84/100 → 第 3 轮: 93/100 ✓
    输出最终结果及完整轨迹
```

## 三者关系

```
┌─────────────────────┐
│       Intro         │  测量标准
│    （评分引擎）      │
└────────┬────────────┘
         │ 提供维度和锚点
         ▼
┌─────────────────────┐
│        IoE          │  引导式改进
│    （比较循环）      │
└────────┬────────────┘
         │ 自动化循环
         ▼
┌─────────────────────┐
│       iloop         │  自主迭代
│    （自动驾驶）      │
└─────────────────────┘
```

## 资源索引

| 内容 | 位置 |
|------|------|
| 逐步上手指南 | [docs/getting-started.md](docs/getting-started.md) |
| 评分维度与权重 | [docs/quality-framework.md](docs/quality-framework.md) |
| 设计理念 | [docs/philosophy.md](docs/philosophy.md) |
| Intro 示例 | [examples/intro-example.md](examples/intro-example.md) |
| IoE 示例 | [examples/ioe-example.md](examples/ioe-example.md) |
| iloop 示例 | [examples/iloop-example.md](examples/iloop-example.md) |
| Intro skill 文件 | [skills/Intro/SKILL.md](skills/Intro/SKILL.md) |
| IoE skill 文件 | [skills/IoE/SKILL.md](skills/IoE/SKILL.md) |
| iloop skill 文件 | [skills/iloop/SKILL.md](skills/iloop/SKILL.md) |

## 学术引用

IoE skill 基于以下学术工作：

> **Confidence Matters: Revisiting Intrinsic Self-Correction Capabilities of Large Language Models**  
> Li et al., arXiv:2402.12563, 2024 · [https://arxiv.org/abs/2402.12563](https://arxiv.org/abs/2402.12563)

本实现在其核心 "If-or-Else" 置信度评估策略基础上，扩展了多模式交互流程和自评 CoT 模板。

## 关于作者

**绿意不息** (Evergreen)，浙江大学学生 · 使用 [Claude Code](https://claude.ai/code) (Anthropic) 构建

这三个 skill 源自日常使用中的观察：LLM 输出存在方差，而结构化自我评估是控制方差最可靠的方式。

## 贡献

详见 [CONTRIBUTING.md](CONTRIBUTING.md)。欢迎 Issue 和 PR。

## 许可

MIT © 2026 绿意不息 (Zhejiang University)
