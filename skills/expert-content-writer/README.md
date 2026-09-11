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
publication-context-builder
      ↓
expert-content-writer
      ↓
publication-qa
```

## 内置 profiles

- `profiles/game-guide.md`
- `profiles/software-tutorial.md`
- `profiles/product-review.md`

Profile 只控制表达与结构，不重新判断事实。

## 使用示例

- `使用 expert-content-writer + game-guide 写这份 publication_context`
- `把这些 verified facts 写成软件教程，不要重新研究`
- `使用 product-review profile，把上下文写成可发布评测`

## 核心边界

Writer 可以重组和改写事实，但不能：

- 补造新事实；
- 提升确定性；
- 泄漏内部来源；
- 违反 `do_not_claim`；
- 删除 required attribution。
