---
name: publication-qa
description: 对最终草稿执行发布前质量门禁，检查来源泄漏、研究过程语言、AI 元话语、无依据事实、置信度膨胀、错误归因、版本/时效问题与编辑一致性，并在不引入新事实的前提下做最小必要修复。
compatibility: 适用于支持 Agent Skills 的 ChatGPT/Agent 环境。可单独检查草稿；若同时提供 publication-context-builder 输出或 canonical facts，可执行更严格的事实一致性 QA。
metadata:
  version: "1.0.0"
  language: "zh-CN"
  domain: "content-engineering"
---

# Publication QA

## 0. 核心目标

这是发布前最后一道门。

它回答两个问题：

1. 这篇内容是否像一个专业作者在直接帮助读者，而不是 AI 在复述研究过程？
2. 草稿中的事实强度，是否超过上游证据真正允许的强度？

## 1. 输入

最低输入：

- final draft。

推荐同时提供：

- `publication_context`；
- canonical facts；
- publication mode；
- domain-specific editorial policy。

没有知识上下文时，可以做语言与泄漏检查，但不能声称完成了完整 factual verification。

## 2. 输出契约

```yaml
status: PASS | FAIL
issues:
  - severity: BLOCKER | MAJOR | MINOR
    type: SOURCE_LEAKAGE
    excerpt: "According to competitor.example..."
    reason: "Internal provenance leaked into published copy."
    action: REWRITE
    suggested_fix: "..."
verification_gaps: []
revised_draft: null
```

若用户要求自动修复，可返回完整修订稿；默认优先最小改动，而不是无必要地重写整篇。

## 3. Blocker 检查

出现以下情况默认 `FAIL`：

### 3.1 Source Leakage

不该公开的：

- competitor URL / domain；
- 内部 source id；
- YouTube timestamp；
- Reddit username；
- research notes；
- “source_01 / evidence_03” 等内部标识；
- 为研究服务、但不应公开的页面标题或来源链。

例外：上游明确标记 `REQUIRE_ATTRIBUTION`。

### 3.2 Unsupported Claim

草稿出现 publication context 中没有、且无法从允许 facts 推导出的新事实。

### 3.3 Certainty Inflation

上游：

> approximately 50%

草稿：

> exactly 50%

上游：

> may occur

草稿：

> always occurs

必须拦截。

### 3.4 Incorrect Attribution

- 把 A 的观点归给 B；
- 把社区观点写成官方结论；
- 把研究推断写成来源原话；
- 删除本应公开的 required attribution。

## 4. Major 检查

### 4.1 Research Meta Language

重点检查但不要只做关键词匹配：

- our research shows；
- we researched；
- our investigation；
- evidence we found；
- multiple sources suggest；
- according to our sources；
- based on the materials provided；
- based on available information；
- the supplied context indicates。

判断标准：

> 这句话是在帮助读者理解主题，还是在向读者讲作者如何做研究？

如果主要是后者，通常应重写。

### 4.2 AI / Process Meta Language

例如：

- based on the prompt；
- from the information provided；
- as an AI；
- I cannot verify；
- the context above；
- the source material。

除非文章主题本身就是研究方法，否则默认移除。

### 4.3 Version / Time Drift

检查：

- 老版本数值写成当前数值；
- 时间敏感价格无日期；
- “currently / latest / now” 无对应时效证据；
- 平台差异被抹平。

### 4.4 Internal Contradiction

同一篇文章不同段落给出不同数字、步骤、条件或结论。

## 5. Minor 检查

包括：

- 重复结论；
- 无信息量的套话；
- 过多“值得注意的是”；
- 泛化式“玩家通常会发现……”但没有支持；
- 为显得权威而添加的空泛措辞；
- 过度 caveat 导致可读性下降。

Minor 不一定阻止发布，但应记录。

## 6. 修复原则

### 可以直接修复

当事实本身有充分支持，只是表达带有 research leakage：

坏：

> According to several sources, the boss enters phase two at around 50% HP.

好：

> At roughly 50% HP, the boss enters its second phase.

### 不可以擅自修复

如果问题是证据不足：

> The quest definitely unlocks at level 15.

而上游 facts 标记 `CONFLICTING`，不能只是改得“更自然”。

应：

- 删除；或
- 按上游 qualifier 降级；或
- 标记 `VERIFY_BEFORE_PUBLISH`。

**QA 不能创造新事实来填洞。**

## 7. Publication Mode Awareness

### expert

来源泄漏标准最严格。普通事实应直接表达。

### editorial

允许必要的来源身份帮助读者判断，但不能复述整个研究过程。

### evidence

required attribution 是正常内容，不得误判为 source leakage。

因此：

> “CDC recommends …”

在 evidence mode 可能是正确归因；

而：

> “During our research, we found a CDC page that says …”

仍然是研究过程泄漏。

## 8. 语义检查优先于禁词

不能简单把 `research / evidence / source` 全部列为禁词。

例如：

> “The study found…”

在学术文章里可能是必须的 attribution。

真正要判断的是：

- 来源是否应公开；
- 这句话是否服务读者；
- 是否泄漏内部工作流；
- 是否超出证据强度。

## 9. Final Gate

发布前逐项确认：

- [ ] 没有内部 URL / source id / research notes 泄漏；
- [ ] 没有无意义的“我们研究发现”；
- [ ] 没有 AI / prompt / supplied context 元话语；
- [ ] 所有关键事实都在允许范围内；
- [ ] qualifier 没丢；
- [ ] required attribution 没丢；
- [ ] 没有把观点/推断写成事实；
- [ ] 没有版本、平台、时间漂移；
- [ ] 全文内部一致。

只有 Blocker 清零，且 Major 已修复或明确接受时，才能 `PASS`。
