# Publication Context Builder Skill

在 Knowledge Store 与 Writer 之间建立隔离层，只把可发布、与当前页面相关的知识交给 Writer。

## Pipeline 位置

```text
Canonical Knowledge
      +
Page Brief
      ↓
content-type-router
      ↓
publication-context-builder
      ↓
Writer Context
      ↓
expert-content-writer
```

## 为什么需要它

与其告诉 Writer：

> 不要输出 competitor URL。

更可靠的方法是：

> Writer 根本看不到 competitor URL。

这个 Skill 默认移除内部 provenance、研究笔记、YouTube timestamp、Reddit username、竞品域名等，只在事实策略要求时保留公开 attribution。

## Router 集成

可接收 `content-type-router` 的：

- `writer_profile`
- `publication_mode`
- `required_modules`
- `optional_modules`
- `context_priorities`

例如 `game-walkthrough` 会要求保留 `sequence / state / direction / count` 等程序语义。

核心原则：

> Strip provenance, not procedure semantics.

## Publication Modes

- `expert`：攻略、教程、how-to、游戏 reference / walkthrough；
- `editorial`：评测、旅游、行业分析；
- `evidence`：新闻、学术、医疗、法律、金融、政策。

## 使用示例

- `使用 publication-context-builder 从 canonical facts 构造 Level 317 的 Writer context`
- `消费 content-type-router 的 routing_decision，只保留 walkthrough 需要的事实`
- `用 evidence mode 构造保留必要 attribution 的写作上下文`
- `不要让 Writer 看到内部来源 URL，只保留允许发布的事实`
