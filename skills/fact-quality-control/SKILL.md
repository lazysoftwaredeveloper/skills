---
name: fact-quality-control
description: 对结构化事实进行去重、冲突检测、来源独立性判断、时效与版本检查、置信度评估，并决定是否允许发布、是否需要限定语或公开归因。适用于 Research → Knowledge 的质量控制层。
compatibility: 适用于支持 Agent Skills 的 ChatGPT/Agent 环境。最佳输入来自 research-to-facts，也可处理其他结构化 claim 集合。若证据不足，必须降级或阻止发布，禁止以多数票替代验证。
metadata:
  version: "1.0.0"
  language: "zh-CN"
  domain: "content-engineering"
---

# Fact Quality Control

## 0. 核心目标

把“提取出来的 claims”转换成“可以被内容系统安全使用的 canonical facts”。

核心链路：

`normalized claims → dedupe → conflict analysis → evidence quality → confidence → publication policy`

本 Skill 不负责写最终文章。

## 1. 输入

推荐输入：

- `statement`；
- `claim_type`；
- `conditions / exceptions / temporal_scope`；
- `provenance`；
- `confidence_hint`。

若缺少 provenance，应明确标记 `evidence_quality: unknown`，不要假装已验证。

## 2. 输出契约

```yaml
canonical_facts:
  - fact_id: fact_001
    statement: "The weapon unlocks at level 15."
    claim_type: FACT
    status: SUPPORTED
    confidence: high
    conditions: []
    exceptions: []
    temporal_scope: null
    evidence_quality: strong
    source_independence: independent
    attribution:
      mode: none
      public_label: null
    publication_policy:
      decision: ALLOW
      qualifier: null
    conflicts: []
    provenance: []
```

## 3. Status

使用以下状态：

- `VERIFIED`：存在强一手证据或权威原始记录，且与命题直接匹配；
- `SUPPORTED`：多个足够可靠、互相独立的证据支持；
- `CONFLICTING`：可靠证据之间存在实质冲突；
- `INSUFFICIENT`：证据不足以支撑该强度的命题；
- `STALE`：事实可能曾经成立，但版本/时间已过期；
- `REJECTED`：命题被可靠证据否定、错误归纳或类型错误。

`VERIFIED` 不等于“永远正确”；仍要保留版本和时效。

## 4. Publication Decision

只能从以下值选择：

- `ALLOW`：可直接作为事实使用；
- `ALLOW_WITH_QUALIFIER`：可发布，但必须保留范围、近似、时间、条件或不确定性；
- `REQUIRE_ATTRIBUTION`：事实可使用，但来源身份对读者理解或可信度是必要信息；
- `VERIFY_BEFORE_PUBLISH`：当前不应进入正文，先补证据；
- `DO_NOT_PUBLISH`：错误、误导、不可修复或不适合发布。

## 5. 工作流

### Step 1 — Atomicity Check

如果一个 statement 包含多个可独立真假的命题，先拆分。

### Step 2 — Deduplication

合并语义相同的 facts，但必须合并 provenance，不能丢来源。

同义表达不是新证据。

### Step 3 — Source Independence

判断多个来源是否真的独立：

- 多个网站复制同一 press release → 非独立；
- 多篇 SEO 文章引用同一个 Wiki → 非独立；
- 官方 patch notes + 实机观察 → 通常独立；
- 同一视频的多个二次转载 → 非独立。

不要用“来源数量”冒充“证据独立性”。

### Step 4 — Conflict Detection

检测：

- 数字冲突；
- 条件冲突；
- 版本冲突；
- 时间冲突；
- 地区/平台差异；
- 分类冲突。

先判断是不是“不同 scope”导致的表面冲突。

例如：

- v1.2 unlock at level 15
- v1.3 unlock after Quest X

应拆成版本化事实，而不是简单投票决定谁对。

### Step 5 — Precision Check

特别审查：

- 精确百分比；
- 精确价格；
- 精确伤害值；
- 时间；
- 排名；
- “always / never / best / only”等绝对词。

来源只支持“大约”时，不得输出精确数字。

### Step 6 — Claim Type Check

确保：

- `OPINION` 没被升级为 `FACT`；
- `COMMUNITY_SENTIMENT` 没被写成客观结论；
- `INFERENCE` 保持为推断；
- `ESTIMATE` 保留近似语义。

### Step 7 — Confidence

使用 `high / medium / low`。

`high` 通常要求：

- 强一手证据，或
- 多个真正独立且一致的可靠证据；
- 关键限定条件已明确；
- 没有未解决冲突。

`medium`：

- 有明确支持，但仍依赖单一来源、近似、解释或有限样本。

`low`：

- 来源弱；
- 冲突未解；
- 只能间接推断；
- 时效未知；
- precision 超出证据。

### Step 8 — Attribution Policy

归因是“发布策略”，不是“真实性评分”。

`attribution.mode`：

- `none`：读者不需要知道来源身份；
- `optional`：可按编辑风格决定；
- `required`：来源身份本身对结论可信度、法律/伦理要求或理解上下文是必要信息。

通常需要公开归因：

- 新闻声明；
- 学术研究结果；
- 医疗、法律、金融等高风险结论；
- 官方政策、规则、统计；
- 某人的观点或声明；
- 无法转化为普遍事实、只能表述为“某主体称”的 claim。

通常不需要公开归因：

- 普通游戏机制；
- 已确认的软件操作步骤；
- 非争议的产品规格；
- 一般 how-to 事实。

## 6. 不可违反的规则

1. **不要多数票判真。**
2. **不要把复制链当独立证据。**
3. **不要因为来源是官方，就把所有主观宣传语当事实。**
4. **不要因为一个来源没提某事实，就把“沉默”视为反证。**
5. **不确定性必须进入 publication policy。**
6. **事实的版本、时间、平台、地区差异优先于强行合并。**
7. **不能靠写作措辞掩盖证据不足。**

## 7. 冲突示例

输入：

```text
A: unlocks at level 15
B: unlocks at level 16
C: unlocks after Quest X
```

若无法解释 scope：

```yaml
status: CONFLICTING
confidence: low
publication_policy:
  decision: VERIFY_BEFORE_PUBLISH
conflicts:
  - "level 15"
  - "level 16"
  - "after Quest X"
```

不要输出“多数来源显示 level 15”，除非真的存在足够独立且高质量的证据支持。

## 8. 交接到下游

推荐下一步：`publication-context-builder`。

传递完整 canonical facts，包括 provenance；下一层会决定哪些字段可以暴露给 Writer。
