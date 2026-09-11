# Publication Context Builder Skill

在 Knowledge Store 与 Writer 之间建立隔离层，只把可发布、与当前页面相关的知识交给 Writer。

## Pipeline 位置

```text
Canonical Knowledge
    ↓
publication-context-builder
    ↓
Writer Context
    ↓
Writer / LLM
```

## 为什么需要它

与其告诉 Writer：

> 不要输出 competitor URL。

更可靠的方法是：

> Writer 根本看不到 competitor URL。

这个 Skill 默认移除内部 provenance、研究笔记、YouTube timestamp、Reddit username、竞品域名等，只在事实策略要求时保留公开 attribution。

## Publication Modes

- `expert`：攻略、教程、how-to；
- `editorial`：评测、旅游、行业分析；
- `evidence`：新闻、学术、医疗、法律、金融、政策。

## 使用示例

- `使用 publication-context-builder 从这批 canonical facts 构造游戏攻略 Writer context`
- `用 evidence mode 构造一份保留必要 attribution 的写作上下文`
- `不要让 Writer 看到内部来源 URL，只保留允许发布的事实`
