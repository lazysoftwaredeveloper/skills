# Fact Quality Control Skill

对结构化 facts 做发布前的知识质量控制：去重、冲突分析、来源独立性、时效、置信度与归因策略。

## Pipeline 位置

```text
research-to-facts
    ↓
fact-quality-control
    ↓
Canonical Knowledge
    ↓
publication-context-builder
```

## 解决的问题

- 三个网站其实都复制同一来源；
- level 15 / level 16 / Quest X 相互冲突；
- 旧版本事实被当成当前事实；
- Reddit 观点被写成客观事实；
- 来源只说“约 50%”，文章却写成 `50.00%`；
- 哪些事实必须公开 attribution，哪些只需内部 provenance。

## 使用示例

- `使用 fact-quality-control 检查这批 facts，冲突的不要直接发布`
- `给每条事实分配 confidence 和 publication policy`
- `判断哪些 claims 必须保留公开来源，哪些可以只保留内部 provenance`
