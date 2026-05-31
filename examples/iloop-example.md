# iloop Example Walkthrough

## Scenario: Fully Automated Code Improvement

**User:**

```
/iloop 用 TypeScript 实现一个 RateLimiter 类，支持：
- 基于令牌桶算法
- 可配置的 refill rate 和容量
- 线程安全（Node.js 环境）
- 完整错误处理
```

**Round 1:**

Implementation with basic token bucket logic. Self-evaluates at 72/100.

**Issues identified:**
- 使用 `Date.now()` 非单调时钟，受系统时间调整影响
- 未校验 `count` 参数（负数/NaN 可破坏桶状态）
- `consume(count > capacity)` 时无限挂起
- 无文档注释

**Round 2:**

Adds input validation, monotonic clock (`performance.now()`), precise wait calculation, factory function. Self-evaluates at 85/100.

**Issues identified:**
- 缺少 `refillInterval` 的零值校验
- 缺少只读快照接口
- 精度补偿（浮点数累积误差）

**Round 3:**

Completes all edge cases, comprehensive JSDoc, getter for remaining tokens. Self-evaluates at 94/100.

**Decision:** Quality threshold met (≥90). Stopping.

---

**Final Result:**

```
## iloop Final Result

**Task:** 用 TypeScript 实现 RateLimiter
**Iterations:** 3
**Final Score:** 94/100

**Result:**

```typescript
/** 令牌桶配置选项 */
export interface TokenBucketOptions {
  capacity: number;
  refillRate: number;
  refillInterval?: number; // 毫秒，默认 1000
}

/** 基于令牌桶算法的速率限制器 */
export class TokenBucket {
  private tokens: number;
  private lastRefill: number;
  private readonly capacity: number;
  private readonly refillRate: number;
  private readonly refillInterval: number;

  constructor(options: TokenBucketOptions) {
    this.capacity = options.capacity;
    this.refillRate = options.refillRate;
    this.refillInterval = options.refillInterval ?? 1000;
    this.tokens = options.capacity;
    this.lastRefill = performance.now();
  }

  /** 尝试消费 count 个令牌，返回是否成功 */
  tryConsume(count: number = 1): boolean {
    if (count <= 0) throw new Error('count must be positive');
    if (count > this.capacity) return false;
    this.refill();
    if (this.tokens < count) return false;
    this.tokens -= count;
    return true;
  }

  /** 阻塞直到有足够令牌可用 */
  async consume(count: number = 1): Promise<void> {
    if (count <= 0) throw new Error('count must be positive');
    if (count > this.capacity) {
      throw new Error('count exceeds bucket capacity');
    }
    while (!this.tryConsume(count)) {
      const needed = count - this.tokens;
      const waitMs = Math.ceil((needed / this.refillRate) * this.refillInterval);
      await new Promise(r => setTimeout(r, Math.max(waitMs, 1)));
    }
  }

  private refill(): void {
    const now = performance.now();
    const elapsed = now - this.lastRefill;
    const added = (elapsed / this.refillInterval) * this.refillRate;
    this.tokens = Math.min(this.capacity, this.tokens + added);
    this.lastRefill = now;
  }

  /** 获取当前可用令牌数（只读快照） */
  get remainingTokens(): number {
    this.refill();
    return this.tokens;
  }
}

/** 创建 TokenBucket 实例，含参数校验 */
export function createTokenBucket(opts: TokenBucketOptions): TokenBucket {
  if (opts.capacity <= 0) throw new Error('capacity must be > 0');
  if (opts.refillRate <= 0) throw new Error('refillRate must be > 0');
  if (opts.refillInterval !== undefined && opts.refillInterval <= 0) {
    throw new Error('refillInterval must be > 0');
  }
  return new TokenBucket(opts);
}
```

**Trajectory:**
- Round 1: 72/100 — 基础实现正确，但存在 `Date.now()` 时钟问题、`count` 未校验、`consume` 可能无限挂起
- Round 2: 85/100 — 改用 `performance.now()`，添加参数验证、精确等待，但仍缺少 `refillInterval` 零值保护和只读接口
- Round 3: 94/100 — 完整边缘覆盖 + JSDoc + getter

**Status:** Quality threshold met
```

---

*Created by **绿意不息** (Evergreen), Zhejiang University · Built with [Claude Code](https://claude.ai/code)*

---

## Factual Verification

The RateLimiter code was audited against token bucket algorithm specifications and Node.js runtime behavior:

| Issue Found | Fix Applied | Source |
|------------|-------------|--------|
| `Date.now()` is non-monotonic | `performance.now()` | Node.js perf_hooks docs |
| No `count` param validation | Added `count <= 0` guard | Input validation best practice |
| `consume(count > capacity)` hangs | Added eager rejection | Fail-fast principle |
| Missing `refillInterval` zero guard | Added validation | Division-by-zero prevention |
| Coarse polling adds latency | Exact wait calculation | Algorithm optimization |
| No public state inspection | Added `remainingTokens` getter | API usability |

---

## Scenario: Plateau Detection

**User:**

```
/iloop 3 写一首关于秋天的诗
```

**Round 1:** 76/100 — 意象丰富但结构松散
**Round 2:** 78/100 — 结构调整了，但新意象略显刻意
**Round 3:** 77/100 — 没有再提升

**Decision:** No improvement for 3 consecutive rounds. Early termination.

**Result:** Returns the best version (Round 2) with plateau note. This is expected — some tasks (especially creative ones) don't always benefit from mechanical iteration.
