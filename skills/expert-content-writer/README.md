# Expert Content Writer

把经过上游筛选的 `publication_context` 写成最终面向读者的专业内容。

它解决的不是“怎么研究”，而是：

> 已经知道什么之后，应该怎样像真正的领域作者一样写出来？

## 推荐工作流

```text
research-to-facts
      ↓
fact-quality-control
      ↓
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

## 内置 profiles

### 游戏站

- `profiles/game-overview.md` — 游戏介绍、How to Play、新手理解、核心循环
- `profiles/game-reference.md` — Mechanics、Controls、Items、Rules、FAQ
- `profiles/game-guide.md` — Boss、Quest、Unlock、Achievement、Collectible
- `profiles/game-walkthrough.md` — Level、Stage、Puzzle 的精确按步解法

### 其他

- `profiles/software-tutorial.md`
- `profiles/product-review.md`

Profile 只控制表达与结构，不重新判断事实。

## 完整游戏站的组合

```text
Homepage / How to Play       → game-overview
Controls / Mechanics / FAQ   → game-reference
Boss / Quest / Unlock        → game-guide
Level 1..N / Puzzle          → game-walkthrough
```

四类页面共用同一个 Knowledge Store。

## 使用示例

- `使用 expert-content-writer + game-overview 写游戏基础介绍`
- `使用 game-reference 写 portal mechanic 页面`
- `使用 game-guide 写 Boss 攻略`
- `使用 game-walkthrough 写 Level 317 的精确解法`
- `使用 software-tutorial 把这些 verified facts 写成教程`

## 核心边界

Writer 可以重组和改写事实，但不能：

- 补造新事实；
- 提升确定性；
- 泄漏内部来源；
- 违反 `do_not_claim`；
- 删除 required attribution；
- 为了文笔重排程序步骤；
- 从最终状态反推不存在的操作路径；
- 为了模板完整硬编 Tips / Common Mistakes / FAQ。
