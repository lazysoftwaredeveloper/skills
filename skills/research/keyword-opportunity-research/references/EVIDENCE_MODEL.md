# Evidence Model

## Evidence Levels

| Level | Name | Meaning | Typical Sources | Can Support |
|---|---|---|---|---|
| E0 | Hypothesis | 分析者提出，尚未独立观察 | 相邻证据推理 | 下一步研究动作 |
| E1 | Single-source clue | 一个页面/站点/结果提到 | vendor page, blog, one forum post | 候选 keyword / pain / workflow |
| E2 | Repeated independent clue | 多个独立来源重复 | unrelated vendors, media, communities | 更强的 use-case / pain hypothesis |
| E3 | Search-intent evidence | 搜索引擎对该 intent 形成明显聚类 | SERP, autocomplete, PAA, related | coherent search intent |
| E4 | User pain evidence | 用户明确用自己的语言描述问题 | Reddit, forums, reviews | real-world pain/workflow |
| E5 | Keyword-market evidence | 定量关键词/流量/竞价数据 | keyword tools, search data | SEO sizing |
| E6 | Paid-market evidence | 可见付费产品/业务 | pricing, paid apps, API, enterprise | monetization signal |

## 使用原则

- Evidence Level 是证据类型，不是线性“分数”。
- 一个 Vendor 页面通常只是 E1。
- SERP 对某意图形成聚类可视为 E3，但不代表高 Volume。
- 一个 Reddit 帖子可作为 E4 类型证据，但样本仍然只有一个。
- 没有正式关键词数据时，不得制造 E5。
- E6 说明有人收费/愿意卖，不自动等于 PMF。

## Confidence

- LOW：主要是 E0/E1。
- MEDIUM：有重复证据与较一致 intent，但用户/商业/量级证据仍不完整。
- HIGH：多个独立来源，加上搜索、用户和/或付费验证。

任何 Confidence 都要列出对应 Step IDs。
