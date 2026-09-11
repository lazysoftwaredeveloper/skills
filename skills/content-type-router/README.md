# Content Type Router

根据 Page Brief 判断页面的用户任务，并选择正确的 `expert-content-writer` profile。

## 推荐位置

```text
Knowledge Store
      +
Page Brief
      ↓
content-type-router
      ↓
publication-context-builder
      ↓
expert-content-writer
      ↓
publication-qa
```

## 当前主要路由

| 页面任务 | Writer profile |
|---|---|
| 游戏介绍、How to Play、新手理解 | `game-overview` |
| 机制、Controls、物品、FAQ | `game-reference` |
| Boss、Quest、Unlock、Achievement | `game-guide` |
| Level、Stage、Puzzle 的精确步骤 | `game-walkthrough` |
| 软件操作教程 | `software-tutorial` |
| 产品评测 | `product-review` |

## 核心原则

Router 只决定“应该怎么写”，不写正文，也不判断事实真假。

对于无法确定的页面类型，返回 `ambiguous`，不要硬猜。
