# Publication QA Skill

最终内容发布前的质量门禁，重点解决“AI 把研究过程和内部来源写进文章”的问题。

## Pipeline 位置

```text
Writer / LLM
    ↓
publication-qa
    ↓
Publish
```

## 检查内容

- competitor URL / domain 泄漏；
- YouTube timestamp / Reddit username / source id 泄漏；
- `our research shows`、`based on supplied information` 等研究元话语；
- AI / prompt / context 元话语；
- unsupported claims；
- certainty inflation；
- required attribution 丢失；
- 版本、时间、平台漂移；
- 全文事实自相矛盾。

## 原则

不是简单“禁词”。

在 evidence mode 中，合法 attribution 必须保留；真正需要删除的是内部研究过程和不必要的 provenance。

## 使用示例

- `使用 publication-qa 检查这篇攻略，发现来源泄漏就修复`
- `对照 publication context 检查有没有新增未支持事实`
- `用 evidence mode 检查这篇文章，必要引用不能被误删`
