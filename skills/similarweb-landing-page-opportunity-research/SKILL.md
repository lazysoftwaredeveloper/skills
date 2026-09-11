---
name: similarweb-landing-page-opportunity-research
description: 从 Similarweb 发现的关键词与 Landing Page 出发，系统挖掘真实需求、自然表达、SERP 意图、需求变量、Demand Graph、竞争缺口与可建站机会，并保留完整但去重的研究证据。
---

# Similarweb Landing Page Opportunity Research

## 目标

给定一个或多个：
- Similarweb 发现的关键词
- Landing Page URL
- 可选：Similarweb 流量、来源词、国家/地区、时间范围、Google Trends 链接或截图

完成从 **Landing Page → Job → Natural Query → SERP → Demand Dimensions → Demand Graph → Opportunity → Site Architecture** 的研究。

本 Skill 的目标不是输出“相关关键词列表”，而是回答：

> 这个 Landing Page 为什么有流量？背后的真实 Job 是什么？这个 Job 能否脱离品牌独立存在？用户还会怎样自然表达？需求由哪些变量决定？能扩成多大的需求空间？哪些节点才是真正值得做的机会？

---

# 核心原则

1. **Landing Page Mining ≠ Keyword Mining，而是 Demand Mining。**
2. **Keyword 是 Job 的语言投影，不是需求本身。**
3. **Search Volume ≠ Capturable Demand。** 品牌词、导航词必须先判断 Demand Ownership。
4. **品牌词不要只做“过滤”。** 继续寻找 Secondary Jobs，再去品牌化为 Generic Job。
5. **Natural Query 必须从用户状态反推，不从 generator / maker / tool / online 等词根机械排列。**
6. **SERP 是需求分类器。** 用结果类型、意图纯度、SERP overlap 判断 Google 如何理解需求和 Cluster Boundary。
7. **找到 Job 后，先找决定结果的变量，再扩词。** 变量构成 Demand Dimensions；变量的真实组合构成 Demand Graph。
8. **Expression Expansion、Constraint Expansion、Job Expansion 必须分开。**
9. **能生成 URL ≠ 应该生成 SEO Page。** Programmatic SEO 只做有自然表达、已观察需求、结果有独立价值的页面。
10. **Build broadly, index selectively, expand from observed demand。**
11. **证据优先于判断。** 所有关键判断必须能追溯到 Landing Page、SERP、官方资料、竞争页面或用户提供的数据。
12. **完整但不重复。** 保留所有研究材料，但同一证据只记录一次；正文只引用，不重复解释。

---

# 适用场景

当用户说类似：
- “分析这个 Similarweb 关键词和 Landing Page”
- “帮我挖这个词”
- “这个页面为什么有流量？”
- “从这个词继续挖需求”
- “看看能不能做站”
- “用 Similarweb Landing Page 方法分析 X”

就使用本 Skill。

如果用户只给关键词，没有 URL，也可以从关键词开始；如果只给 URL，则先从页面反推 Job 和 Natural Query。

---

# 输入

最小输入：

```text
Keyword: <seed keyword>
Landing Page: <url，可空>
```

可选输入：

```text
Country: US / Global / ...
Time range: 30d / 12m / ...
Similarweb traffic:
Similarweb keyword share:
Google Trends:
User notes:
```

不要因为缺少 Similarweb 精确数字就停止研究。无法验证的数据标记为 `Unknown / Need user data`，不要编造。

---

# 强制研究流程

## Step 0 — 建立 Research Ledger

研究开始就维护一个证据账本。每条证据只记录一次。

字段：

| ID | Type | Query / URL | Observation | Supports | Source |
|---|---|---|---|---|---|

Type 只使用必要类别：
- Landing Page
- SERP
- Competitor
- Official
- Trend
- Similarweb/User Data

要求：
- 所有实际搜索过的重要 Query 都进入 Ledger。
- 所有最终使用到的竞争页面都进入 Ledger。
- 无价值、重复、明显噪声结果不逐条罗列；用一条 `Discarded` 汇总即可。

---

## Step 1 — Landing Page 解剖

先不讨论流量大小，也不直接扩关键词。

提取：

```text
Input → Action → Output
```

同时记录：
- 页面类型：Tool / Game / Content / Directory / Product / Marketplace / Other
- First-screen value proposition
- 核心交互
- 是否需要登录
- 是否立即完成 Job
- 页面是否强依赖品牌 / 平台 / 第三方实体
- 明显差异化功能
- 页面是否老旧、停更、deprecated、clone、demo、开源项目主页等

输出一句 Job Statement：

> When [situation/input], user wants to [action] so that [result].

不要用营销语言。

---

## Step 2 — Demand Ownership 分类

判断 Seed Keyword 属于：

- Generic Demand
- Category Demand
- Brand / Entity Demand
- Navigational Demand
- Informational Demand
- Tool / Action Demand
- Mixed

然后回答：

> 这个搜索需求是谁拥有的？

给出：

```text
Demand Independence: High / Medium / Low
Brand Dependency: High / Medium / Low
Capturable Demand: High / Medium / Low / Unknown
```

### 如果是 Brand / Entity

不能只写“品牌词，过滤”。必须继续：

```text
Brand / Entity
→ 用户为什么还会继续搜索？
→ Secondary Jobs
→ 去品牌化
→ Generic Job
```

例如：

```text
Wordle
→ Wordle solver
→ 5 letter word solver
→ 5 letter word finder
→ word pattern solver
```

对原品牌主词可以 `Filter`，但要标记为：

```text
Primary keyword: Filter
Secondary-job seed: Keep
```

---

## Step 3 — 生成 Natural Queries

不要使用机械词根排列组合作为主方法。

必须从以下来源生成：

1. 用户任务如何自然描述
2. 用户已经掌握的信息
3. 用户缺少的信息
4. 用户处于什么状态 / 卡在哪里
5. 用户想得到什么结果
6. 用户可能加入什么约束条件
7. 竞争页面实际使用的语言
8. SERP 中出现的自然表达

先生成 5–15 个高价值候选，不追求数量。

按三类标记：

### A. Expression Expansion
同一个 Job，不同说法。

### B. Constraint Expansion
同一个 Job，不同输入、条件、状态。

### C. Job Expansion
相邻但不同的 Job。

禁止混成一张无结构关键词表。

---

## Step 4 — SERP Validation

对核心 Seed + 主要 Natural Queries 实际搜索。

每个 Query 至少判断：
- Top results 主要是什么页面类型
- 是否工具优先
- 是否品牌/官方站占据
- 是否大量 UGC / Reddit / YouTube / 内容页
- 是否有小站、niche site 进入
- 是否出现 programmatic pages
- 是否存在明显 feature gap

计算定性 `SERP Intent Purity`：

```text
Very High / High / Mixed / Low
```

示例：

```text
8/10 = online tool → Very High tool intent
3 tools + 3 articles + 2 forums + 2 products → Mixed
```

不要假装这是精确统计；如果没有完整 Top 10，就写“sampled SERP”。

---

## Step 5 — Cluster Boundary

关键词相似不等于同一需求。

使用两项共同判断：

1. **Job overlap**
2. **SERP overlap**

标记：

```text
Same Cluster
Adjacent Cluster
Different Job
Brand-derived only
```

典型判断：

```text
minify html ≈ html minifier → Same Cluster
word finder ≠ word unscrambler → Different Job
```

每个 Cluster 只指定一个 canonical intent，不重复造页面。

---

## Step 6 — 提取 Demand Dimensions

这是本 Skill 的核心步骤之一。

问：

> 什么变量变化时，结果也会变化？用户会把哪些变量写进搜索框？

例如 Word Finder：

```text
length
starts_with
ends_with
contains
excludes
position
vowels
repeated_letters
dictionary
```

例如 Image Tool：

```text
input_format
output_format
background
size
style
batch
quality
platform
```

例如 Calculator：

```text
input units
output units
formula/method
industry context
region/standard
```

把变量分成：
- Core Dimensions：用户经常直接搜索
- Secondary Dimensions：更多用于产品筛选
- Non-search Dimensions：功能有用，但通常不是 SEO Landing Page

---

## Step 7 — 构建 Constraint Space

将变量组合成理论空间，但不等于生成页面。

格式：

```text
Dimension A × Dimension B × Dimension C
```

然后抽取“自然组合”：

```text
5 letter words ending in e
5 letter words second letter a
5 letter words with a and r
5 letter words starting with s ending in e
```

对每类组合判断：
- Natural expression?
- Observed SERP / demand evidence?
- Useful result set?
- Unique user state?

只有满足前 3 项的组合才进入 SEO Candidate Pool。

---

## Step 8 — 构建 Demand Graph

Demand Graph 必须同时包含 4 个方向：

### 1. Upward abstraction
从具体词向真正 Root 抽象。

```text
5 letter word finder
→ word finder
→ words matching constraints
```

### 2. Expression branches
同一 Job 的自然表达。

### 3. Constraint branches
同一 Job 的真实条件组合。

### 4. Adjacent jobs
用户前一步 / 后一步 / 替代动作。

输出树或 Mermaid/文本树，避免无结构大表。

---

## Step 9 — Competition & Gap Analysis

不要只看页面“丑不丑”。

对 3–8 个代表性竞争者比较：

- Core Job completeness
- Interaction speed
- Input modes
- Output modes
- Batch
- Privacy / client-side
- Mobile
- Filtering / sorting
- Saved state / share
- Natural-language input
- Data quality
- Niche specialization
- SEO architecture
- Content depth
- Brand authority
- Backlink / open-source / community advantage（能观察到时）

输出 Gap 时必须区分：

```text
Product Gap
UX Gap
Content/Data Gap
SEO Coverage Gap
Distribution/Authority Gap
```

“页面很旧”本身不是 Gap。

---

## Step 10 — Opportunity Scoring

使用 1–5 分，但不要制造假精度。每项给一句理由。

| Dimension | Score |
|---|---:|
| Demand Reality | 1–5 |
| Demand Independence | 1–5 |
| Intent Purity | 1–5 |
| Capturable Demand | 1–5 / Unknown |
| Competition | 1–5，5=容易 |
| Product Gap | 1–5 |
| Build Cost | 1–5，5=容易 |
| Long-tail Breadth | 1–5 |
| Programmatic Potential | 1–5 |
| Repeat Usage | 1–5 |
| Monetization | 1–5 |
| Risk | 1–5，5=低风险 |

最后只给一个明确决策：

- `Build standalone`
- `Build as vertical`
- `Add to toolset`
- `Keep as seed`
- `Monitor`
- `Reject`

同时写一句：

> 为什么不是其它决策？

---

## Step 11 — Site Potential & Architecture

不要只评价一个词，要判断它适合：

```text
Single page
Tool cluster
Vertical site
Programmatic directory
Content + tool hybrid
Not worth building
```

如果是 Tool / Vertical，输出最小 Site Architecture。

要求：
- 只列有明确 Job 的一级/二级页面。
- 不因为 URL 可生成就列出来。
- 标记 `Core / Phase 2 / Optional`。

---

## Step 12 — Progressive Indexing Plan

如果存在 Programmatic SEO：

### 原则

```text
Tool capability ≠ SEO landing page
```

工具可以支持任意组合，但 SEO 页面必须选择性开放。

### SEO Candidate 进入索引的必要条件

```text
Natural Expression
× Observed Demand
× Useful Result Set
```

### 分批方式

- Phase 1：最自然、最强需求 Cluster
- Sitemap 仅包含可索引正式页
- 未验证组合不生成永久 SEO URL，或明确 noindex
- 观察 GSC：Queries / Impressions / Indexed / Ranking / CTR
- 根据真实 Query 和使用行为扩 Phase 2

不要把“只提交部分 Sitemap”当成索引控制手段。

---

# 强制输出格式

输出要 **证据完整，但正文去重**。严格分为两层：

## Layer A — Decision Brief（先给，必须短）

最多包含：

1. **一句话结论**
2. **Seed 判断**：Generic / Brand / Tool / ...
3. **真正 Root Job**
4. **最值得继续挖的 3–5 个 Cluster**
5. **机会决策**
6. **最大风险 / 最大不确定性**

目标：用户 60 秒能看懂。

---

## Layer B — Research Pack（完整证据）

固定按以下顺序：

### 1. Landing Page Anatomy

| Field | Finding |
|---|---|

### 2. Job & Demand Ownership

简洁描述 + 分类。

### 3. Natural Query Map

按 Expression / Constraint / Job 三组展示，只保留有意义的词。

### 4. SERP Evidence

| Query | Dominant intent | Representative results | Interpretation |
|---|---|---|---|

不要为同一结论重复列 10 个近似结果；3–5 个代表性结果足够，其余写数量或模式。

### 5. Cluster Boundaries

| Cluster | Canonical intent | Include | Exclude |
|---|---|---|---|

### 6. Demand Dimensions

| Dimension | Values/examples | Search-facing? | Evidence |
|---|---|---|---|

### 7. Demand Graph

必须给结构树。

### 8. Competitor / Gap Map

只列代表竞争者，避免全量抄 SERP。

### 9. Opportunity Scorecard

给分 + 一句话原因。

### 10. Site / Product Architecture

只输出最小可行结构。

### 11. Indexing Plan

如果不适合 programmatic SEO，明确写 `Not applicable`。

### 12. Research Ledger

列出本次所有真正使用过的研究材料。

字段：

| ID | Type | Query / URL | Key observation | Used for |
|---|---|---|---|---|

这是“所有研究材料”的最终留档区。

### 13. Discarded / Unverified

用极简列表记录：
- 搜过但噪声太大的方向
- 没有足够证据的假设
- 需要用户补 Similarweb 数据才能判断的部分

不要静默丢掉。

---

# “足够多但尽可能少”的压缩规则

必须遵守：

1. **证据不丢，解释去重。** 同一个事实只解释一次，后续引用 Evidence ID。
2. **保留代表结果，不保留 SERP 噪声。** 同类页面最多展示 3–5 个代表样本。
3. **关键词只保留能证明结构的。** 不输出几百个机械变体。
4. **用变量表达规模，用样例表达自然语言。** 例如写 `Position × 26 letters`，而不是把 130 个词全部打印出来。
5. **只有用户明确要求完整 keyword export 时，才输出全量关键词。**
6. **所有被用于最终判断的材料必须进入 Research Ledger。**
7. **所有重大不确定性必须显式写出，不能用模糊措辞隐藏。**
8. **不要为了显得研究充分而重复标题、摘要、结论。**

---

# 搜索与证据规则

- 当前 SERP、产品状态、竞争情况、趋势、法律/IP 风险、平台政策等必须联网验证。
- Landing Page 必须实际打开，不根据域名猜页面内容。
- 重要 Query 必须实际搜索，不靠模型臆测 SERP。
- 用户给出的 Similarweb 数字视为用户数据，可直接引用，但不要伪造缺失指标。
- Google Trends 如果无法读取精确数值，只能做可验证的定性结论；不得编造趋势数字。
- 品牌/IP 风险只做机会筛选层面的风险提示，不冒充法律意见。
- 对“SERP overlap”如果没有完整结果数据，使用 `High / Medium / Low` 定性，不造百分比。

---

# 停止条件

满足任一条件可以停止向下扩：

1. 已找到 Generic Root Job，且 Demand Graph 的主要方向开始重复。
2. 新 Query 只是在机械替换字母、数字、地点、格式，没有新的 Job 或自然表达。
3. SERP 已明显进入另一个 Job，转为 Adjacent Cluster，不继续混挖。
4. 缺少关键外部数据，继续推演只会增加猜测。
5. 已足够支持 Build / Keep / Reject 决策。

停止时必须说明：

> 为什么停在这里，以及下一步如果继续，最值得验证什么。

---

# 最终质量检查

提交前逐项确认：

- [ ] 是否真正打开了 Landing Page？
- [ ] 是否写清 Input → Action → Output？
- [ ] 是否判断 Demand Ownership？
- [ ] 如果是品牌词，是否挖了 Secondary Jobs？
- [ ] Natural Query 是否来自真实用户状态，而非机械扩词？
- [ ] 是否实际验证核心 SERP？
- [ ] 是否区分 Same Cluster / Adjacent Job？
- [ ] 是否提取了 Demand Dimensions？
- [ ] 是否形成 Demand Graph，而非关键词堆？
- [ ] 是否分析了“为什么这个 Landing Page 有流量”？
- [ ] 是否区分 Demand Validation 和 Opportunity Validation？
- [ ] 是否给出明确 Build / Keep / Reject？
- [ ] 是否判断 Single Page / Toolset / Vertical？
- [ ] 若 programmatic SEO，是否给 Progressive Indexing？
- [ ] Research Ledger 是否包含所有真正用于判断的材料？
- [ ] 是否去除了重复信息和无价值 SERP 噪声？

---

# 调用示例

用户：

```text
用 Similarweb Landing Page Opportunity Research 分析：
keyword: 5 letter word finder
landing page: https://example.com/...
```

或者：

```text
按 Landing Page 需求挖掘方法分析 headcanon generator。
```

或者：

```text
继续深挖这个 seed，不要停在品牌词：wordle
```

默认行为：完整执行研究流程，并输出 `Decision Brief + Research Pack`。
