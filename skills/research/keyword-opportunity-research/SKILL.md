---
name: keyword-opportunity-research
description: 系统研究关键词或网站，沿真实证据链从 Seed → SERP/页面 → 新词/新站 → 用户痛点 → 垂直长尾 → SaaS 机会，并把完整、可审计的研究过程直接输出在当前会话中。适用于关键词研究、网站需求地图、SERP 探索、竞争与搜索意图分析、垂直长尾挖掘、小而美 SaaS 机会发现。
compatibility: 适用于支持 Agent Skills 的 ChatGPT/Agent 环境。需要联网搜索才能执行完整 SERP/页面研究。若某类搜索界面数据不可用，必须标记缺失，禁止臆造。
metadata:
  version: "1.1.0"
  language: "zh-CN"
  domain: "keyword-research-saas-opportunity"
---

# Keyword Opportunity Research

## 0. 指令优先级

用户当前明确指令优先于本 Skill。若用户要求缩小范围、停止某分支、只研究网站、只研究关键词、不要使用某个数据源或改变输出方式，应服从用户。

## 1. 核心目标

从用户给出的一个关键词或网站开始，执行真实、可追溯、证据驱动的探索：

`seed → observed evidence → clue → hypothesis → query/page → validation → vertical job/pain → opportunity or rejection`

目标不是机械组合关键词，也不是尽快产出“听起来合理”的机会，而是找到：

- 自然搜索语言；
- 明确的用户任务（Job to Be Done）；
- 专业或垂直场景；
- 普通通用工具处理不好的失败模式；
- 重复性工作流；
- 商业/付费信号；
- 可扩展的长尾关键词簇；
- 小而美 SaaS / utility / workflow 产品机会。

## 2. 输出方式

本 Skill 只有一种模式：

### Conversation Mode

- 所有研究过程直接输出在当前会话中。
- 每个真实研究动作都必须形成一个 Step。
- 长研究可以按阶段批量输出，但不能省略中间动作。
- 必须让人工审核者能从最终结论一路回溯到最初 Seed。
- 研究日志以当前会话为唯一输出载体。
- 不要求用户代替研究者手动搜索，除非用户主动选择手动协作，或当前环境确实无法搜索。

## 3. 不可违反的证据规则

### 3.1 Evidence / 分析 / 决策必须分开

**Observed Evidence**
- 来源实际出现的内容。
- Query、SERP Title、Snippet、Autocomplete、PAA、Related Searches、页面 Phrase、用户原话、Pricing 文案等保持来源原文。
- 禁止翻译、润色、同义改写后再冒充原始证据。

**分析**
- 研究者对证据的解释。
- 统一使用中文。
- 可以抽象 Job、Pain、Workflow、Audience、Failure Mode，但必须明确这是分析。

**决策**
- `继续 / 排队 / 验证 / 降级 / 淘汰`
- 必须说明为什么。
- 必须指出下一步具体动作为什么从当前证据自然产生。

### 3.2 禁止来源漂移

必须标记发现方式。尤其区分：

- `ORGANIC_SERP`
- `PAID_SERP`
- `AUTOCOMPLETE`
- `PAA`
- `RELATED_SEARCH`
- `SITE_NAVIGATION`
- `INTERNAL_LINK`
- `TARGETED_SITE_SEARCH`
- `DIRECT_URL`
- `EXTERNAL_REFERENCE`
- `USER_PROVIDED`
- `ANALYST_HYPOTHESIS`

规则：

- 站内探索发现的页面，不能写成“原 Query 的自然 SERP 结果”。
- `site:` 搜索只能说明站内存在相关内容，不能证明原关键词自然排名。
- Vendor blog 中出现一个词，只是 clue，不能自动升级为搜索需求。
- Search result rank 只能按实际所用搜索界面返回顺序记录；若不是 Google 浏览器原生 SERP，要写清 `Search interface`。
- 若当前搜索工具不提供 Autocomplete / PAA / Related Searches，写 `NOT_AVAILABLE_IN_CURRENT_INTERFACE`，禁止补造。

### 3.3 历史完整性

后续发现不能反向修改早期 Step，使研究路径看起来“本来就很明显”。

如果旧判断被推翻：

1. 新建 `VALIDATION` / correction Step；
2. 引用旧 Step；
3. 记录新证据；
4. 更新当前状态；
5. 保留旧 Step 的原始判断。

## 4. Step 类型

使用稳定类型：

- `SERP_EXPLORATION`
- `SITE_EXPLORATION`
- `PAGE_REVIEW`
- `VALIDATION`
- `USER_EVIDENCE`
- `KEYWORD_DATA`

一个有意义的独立研究动作 = 一个 Step。不要把多个独立 Query 合并成一个 Step，只因为它们支持同一个结论。

## 5. Evidence Level

使用以下层级，禁止跳级：

- `E0 — Hypothesis`：分析者提出、尚无独立观察。
- `E1 — Single-source clue`：一个页面/站点/结果提到。
- `E2 — Repeated independent clue`：多个互不依赖来源重复。
- `E3 — Search-intent evidence`：SERP / Autocomplete / PAA / Related Search 对意图形成聚类。
- `E4 — User pain evidence`：Reddit、论坛、评论、社区中用户明确描述问题。
- `E5 — Keyword-market evidence`：Volume、KD、CPC、排名等定量关键词数据。
- `E6 — Paid-market evidence`：Pricing、付费 App/API、商业客户等可见付费市场。

注意：

- E3 不代表高搜索量。
- E6 证明市场有人收费，不代表已证明 PMF。
- 没有关键词工具数据时，不得编造 Volume/KD/CPC。
- SERP 插件显示的估算流量不能自动当作 E5；除非来源与方法被明确记录。

详见 `references/EVIDENCE_MODEL.md`。

## 6. Keyword Seed 工作流

当 Seed 是关键词时：

### Phase A — Raw Intent Baseline

第一步默认搜索**原始 Seed**，不要一开始就加 AI / online / tool / free / quotes 等修饰词，除非 Seed 本身包含。

目标：

- 看母词 SERP 到底被 Google/搜索界面理解成什么；
- 判断是 Tool / Information / Brand / Marketplace / Community / Mixed intent；
- 记录成熟大站与专用小站；
- 抽取页面中反复出现的 input / use case / outcome / workflow；
- 发现 Generic modifier 与真正垂直信号。

### Phase B — Candidate Branches

候选分支优先来自真实证据：

- Autocomplete / PAA / Related Searches；
- SERP Title / Snippet；
- 已打开页面的 exact phrases；
- 用户论坛语言；
- 明确 use case；
- 下游工作流；
- Generic tool 的失败模式。

分支不能无来源地突然出现。没有来源的想法必须标 `E0 / ANALYST_HYPOTHESIS`。

### Phase C — Controlled Expansion

每个新 Query 必须记录 `Query Rationale`。

常见 modifier 含义：

- `AI`：technology filter
- `image` / `photo`：input object
- `tool` / `online`：product-form
- `product`：use case
- `batch`：workflow / scale
- `for <industry>`：vertical
- `without / preserve / fix`：failure/pain language
- quotes：exact phrase / market naming validation
- `site:`：targeted site mining，不是 organic-ranking validation

如果某个 modifier 解释不了为什么出现，不要使用。

### Phase D — Move from Feature to Job

不断问：

- 用户为什么要做这个动作？
- 完成这个动作之后还要做什么？
- 哪一步如果失败会造成金钱/时间损失？
- 谁会反复做？
- 有没有平台/工艺/文件格式/合规限制？
- 普通通用工具会在哪些对象上失败？

优先从：

`generic feature → use case → workflow → vertical → painful constraint → commercial job`

而不是：

`generic feature → 更多 generic modifier`

## 7. Website Seed 工作流

当 Seed 是网站时，不能直接把网站名字当关键词搜完就结束。

### Phase A — Site Coverage Audit

先盘点：

- Homepage / Header navigation
- Footer
- Tools / Features
- Use Cases / Solutions
- Industries / Audiences
- Pricing
- API / Integrations
- Blog / Guides
- Help / Docs
- Sitemap / page inventory（可获取时）
- 重要 Internal Links
- 必要时 Targeted `site:` 补漏

记录 Coverage：

- `HIGH`
- `MEDIUM`
- `LOW`

### Phase B — Page Inventory

记录重要页面：

| 页面 Title（原文） | URL | Page Type | Important Terms（原文） | 分析 | Follow-up? |
|---|---|---|---|---|---|

### Phase C — Vendor Phrase → Candidate Query

网站原文不是搜索语言。必须显式经历：

`VENDOR_PHRASE → CANDIDATE_QUERY → SERP_TESTED → VALIDATED_QUERY / REPHRASED / REJECTED_AS_QUERY`

绝不能静默把 Vendor wording 改写成“看起来像关键词”的词。

### Phase D — Open Web Validation

用自然 Query 在开放 Web 验证：

- 是否多个站点提供同类功能；
- SERP 是否形成独立 intent；
- 用户是否使用相似语言；
- 是否存在付费市场；
- 是否有更自然的 query wording。

## 8. SERP Step 必须记录什么

每个 `SERP_EXPLORATION` 至少包含：

### Metadata

- Step ID
- Parent Step(s)
- Trigger
- Objective
- Hypothesis
- Exact Query
- Query Rationale
- Search engine/interface
- Locale/market（若已知）
- Date（若可得）
- Operators / quotes

### Organic / returned results

尽量记录实际检查的前 10–20 个结果；若接口只返回更少，就如实记录。

| Rank | Title（原文） | Domain | URL | Snippet / Description（原文） | Result Type | 分析 |
|---:|---|---|---|---|---|---|

### Search features

若可用分别记录：

- Autocomplete
- PAA
- Related Searches / People Also Search For
- Ads（仅在商业意图有价值时）

禁止把自己推测的词填到这些搜索功能中。

### Branch decision

| Candidate | Source | Evidence | Status | 原因 | Next / Revisit |
|---|---|---|---|---|---|

## 9. Page Review 必须记录什么

对于每个重要页面记录：

- URL
- Page title（原文）
- Site
- Discovery method
- Source Step
- 为什么打开

然后：

| Section / Location | Observed Feature / Claim（原文或原始事实） | Exact Phrase（原文） | Phrase Type | 抽象 Job | 分析 |
|---|---|---|---|---|---|

Useful phrases：

| Phrase（原文） | Type | Evidence Level | 分析 | Candidate follow-up |
|---|---|---|---|---|

Phrase Type 常用：

`FEATURE / PAIN / OUTCOME / WORKFLOW / AUDIENCE / INDUSTRY / INPUT / OUTPUT / FORMAT / CONSTRAINT / FAILURE_MODE / COMMERCIAL_SIGNAL / INTEGRATION / PRICING_SIGNAL`

## 10. 用户证据

当某个方向已经形成明确假设，优先寻找真实用户语言：

- Reddit
- forums
- community
- reviews
- professional groups
- help discussions

不要只收集“有人推荐什么工具”；优先记录：

- 用户输入是什么；
- 为什么当前方法失败；
- 失败成本是什么；
- 是否反复发生；
- 用户已经尝试了什么；
- 用户如何自然描述问题。

保持用户原话原文；分析用中文。

## 11. 机会判断

一个候选机会至少从以下维度分析：

- 痛点强度
- 是否重复发生
- 商业/职业用户比例
- 是否存在明确失败成本
- Generic 工具是否处理不好
- 搜索意图是否独立
- 竞争密度
- 是否能形成多个自然 Landing Pages
- 是否有付费市场信号
- 技术可实现性
- 滥用 / 法务 / 平台风险
- 与更强候选是否重复

不要把这个定性框架伪装成精确统计模型。

建议最终状态：

- `PROMISING`
- `DEPRIORITIZED`
- `REJECTED`
- `REVISIT`

Confidence：

- `LOW`
- `MEDIUM`
- `HIGH`

## 12. 优先寻找的“深层机会”模式

优先注意：

- “做完生成以后怎么修”
- “只改局部，不重做全部”
- “提高分辨率但不能改变文字/Logo/商品”
- “批量一致性”
- “从消费者文件变成生产可用文件”
- “平台/工艺规格”
- “专业文件格式”
- “某种材质/内容是 Generic AI 的失败模式”
- “供应商输入质量很差，需要标准化”
- “人工每单都在重复修复”
- “简单功能 + 垂直 workflow 能产生更高价值”

不要因为模式看起来熟悉就直接生成机会；仍然必须从当前研究证据验证。

## 13. 停止条件

研究不是无限发散。满足以下任一条件可开始收敛：

1. 至少一个方向已拥有较完整 evidence trail，通常包括 E2/E3，并尽量有 E4 或 E6；
2. 主要分支均已验证且明显被淘汰；
3. 进一步搜索只重复已有信息，新增 clue 很少；
4. 用户指定时间/深度/范围已达到。

若缺 E5，明确写：

`尚未完成关键词量级验证；当前竞争/需求判断为定性。`

## 14. Conversation Mode 输出协议

每完成一个阶段，在会话中输出：

### A. Step Log
按真实时间顺序输出 Step。

### B. Current Research State
维护轻量状态表：

| Root / Clue | First Seen | Evidence | Status | 最新判断 |
|---|---|---|---|---|

### C. Discovery Graph
记录重要转换：

| From | Relation | To | Step | Evidence / Trigger | Status | 分析 |
|---|---|---|---|---|---|---|

### D. Branch Queue
明确：
- 下一步追什么
- 暂存什么
- 淘汰什么
- 原因

最终才输出机会结论。

使用 `references/OUTPUT_TEMPLATES.md` 中的格式。

## 15. 最终报告

只写能回链到 Step history 的结论。

每个 Opportunity 包含：

- Opportunity / Root
- Target user
- Job to Be Done
- Key pain
- Keyword clusters
- Evidence trail（Step IDs）
- Evidence levels
- Current competition assessment
- Monetization evidence
- Product concept
- Open questions
- Rejected alternatives
- Confidence

最后追加：

### Research Gaps

说明仍未验证的：
- volume / KD / CPC
- 真实付费转化
- 技术可行性
- 平台依赖
- 用户样本
- 竞争者遗漏


## 16. 最终自检

完成前逐项检查：

- [ ] 是否从用户真实 Seed 开始？
- [ ] 每个新 Query 都有可追溯来源或明确 E0？
- [ ] 是否保存了真实 Title / Snippet / Phrase，而非分析者改写？
- [ ] 是否把 Site Exploration 当成 Organic SERP？
- [ ] 是否把 `site:` 搜索误当自然排名？
- [ ] 是否混淆 Vendor Phrase 与自然 Query？
- [ ] 是否编造 PAA / Autocomplete / Related Search？
- [ ] 是否编造 Volume / KD / CPC？
- [ ] 是否记录未追/淘汰的分支？
- [ ] 是否保留历史判断，而不是按最终答案重写故事？
- [ ] 最终 Opportunity 是否能回链到具体 Step？
- [ ] 证据不足时是否明确写不确定性？
