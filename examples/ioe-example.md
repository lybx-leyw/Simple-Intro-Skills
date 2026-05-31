# IoE Example Walkthrough

## Scenario: Iterative Refinement of an Analysis

**User:**

```
/IoE 分析为什么微服务架构在中小型团队中经常失败
```

**Round 1:**

Claude produces an answer covering: communication overhead, DevOps burden, premature decomposition.

> **Claude asks:** 继续优化？(yes=接受并自评 / no=下一轮 / stop=停止)

**User:**

```
no
```

**Round 2:**

Claude reviews previous answer and produces a revised version. Now includes: organizational Conway's Law implications, cognitive load per developer, database complexity. The answer is different — drift detected.

> **Answer drift detected.**  
> no=继续 / refine=精炼 / stop=停止

**User:**

```
no
```

**Round 3:**

Claude produces yet another version. This one focuses more on: lack of observability infrastructure, contract testing overhead, and the "distributed monolith" anti-pattern.

> **Answer drift detected.**  
> no=继续 / refine=精炼 / stop=停止

**User:**

```
refine
```

**Refinement Round:**

Claude consolidates all three answers, synthesizing the best points from each:

```
## ANSWER ##
微服务在中小型团队失败的核心原因（依据 Team Topologies、Martin Fowler、Sam Newman 及多项行业案例研究）：

1. 认知负载失衡——微服务增加无关认知负载（网络通信、分布式事务、服务边界），在小团队中与业务价值负载竞争有限的认知资源
2. 基础设施成本——K8s + 可观测性堆栈对 <10 人团队消耗约 10-25% 的工程能力，相当于 0.5-1 个全职人力的机会成本
3. 分布式单体陷阱——按数据库表而非领域边界拆分，导致同步调用链、协调部署、共享数据库，兼具分布式和单体的缺点
4. 契约测试缺失——服务间接口变更缺乏消费者驱动契约保护，在小团队中虽影响较小，但随服务增长升级为关键风险
5. 康威定律反噬——将微服务强加于紧密协调的组织结构上，会产生架构与组织的错配

结论：中小型团队应先做模块化单体（InfoQ 2025 调查显示 42% 的微服务采纳者本应使用），在出现独立部署需求时用绞杀者模式逐步提取服务。
## END ##
```

> **Stable. yes=接受并自评 / no=继续 / stop=停止**

**User:**

```
yes
```

**Self-Evaluation (CoT):** Claude scores each dimension, produces Overall: 85/100.

> **还需优化？(yes=重新循环 / stop=结束)**

**User:**

```
stop
```

---

---

*Created by **绿意不息** (Evergreen), Zhejiang University · Built with [Claude Code](https://claude.ai/code)*

---

## Scenario: Auto Mode

**User:**

```
/IoE auto 3 用中文解释 JWT 认证流程
```

Claude runs 3 rounds silently:

1. Round 1: JWT structure (header, payload, signature) + sign flow + verify flow
2. Round 2: Adds security considerations (alg:none 攻击防护、算法混淆、声明验证）
3. Round 3: Adds storage best practices (HttpOnly Cookie vs localStorage) + revocation strategies

**Result:**

```
稳定 ##JWT认证流程：签发→传输→验证（标准三步流程）。关键：签名防篡改、条件无状态（短过期且无吊销需求时可无状态）、强制验证alg和exp/iss/aud声明、敏感数据勿放payload（仅base64编码非加密）、HttpOnly+Secure Cookie存储。##
```

No drift — the answer expanded but maintained consistency.

**Note:** Refresh token 属于 OAuth 2.0（RFC 6749）的扩展模式，并非 JWT 规范本身。标准 JWT 认证为签发→传输→验证三步。

---

## Factual Verification

The claims in the microservices example were verified against these sources:
- **认知负载**: Team Topologies (Skelton & Pais, 2019), Martin Fowler (MonolithFirst)
- **基础设施成本**: CNCF case studies, Viva IT (2026) — K8s migration exit
- **分布式单体**: Sam Newman (Building Microservices), InfoQ 2025 survey (42% regretted)
- **契约测试**: Pact documentation, OWASP, Alibaba Cloud tech report
- **康威定律**: Li et al. (2025, arXiv:2501.17522), ThoughtWorks Tech Radar
- **模块化单体优先**: Martin Fowler, Sam Newman, Amazon Prime Video 2023 case study

The JWT example was verified against RFC 7519, OWASP JWT Cheat Sheet, jwt.io, and Auth0 documentation. Key correction: refresh tokens are OAuth 2.0 (RFC 6749), not part of JWT spec.

---

## Pattern: Oscillation Detection

When alternating A→B→A→B:

```
振荡 [A]↔[B] 强烈建议refine
```

This means the model is genuinely uncertain about which framing is correct. `refine` forces a synthesis that usually resolves the conflict.
