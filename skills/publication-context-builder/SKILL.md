---
name: publication-context-builder
description: 从经过质量控制的知识库中，为具体文章、攻略、教程或页面构造“发布上下文”：只选择与任务相关且允许发布的事实，并默认移除内部 provenance、竞品 URL、研究笔记和证据噪音。适用于 Knowledge → Writer 的隔离层。
compatibility: 适用于支持 Agent Skills 的 ChatGPT/Agent 环境。最佳输入来自 fact-quality-control。可用于游戏、软件、SEO、旅游、产品评测、研究型内容等领域，并通过 publication mode 控制 attribution 暴露。
metadata:
  version: "1.0.0"
  language: "zh-CN"
  domain: "content-engineering"
---

# Publication Context Builder

## 0. 核心目标

让 Writer 看到“需要写的知识”，而不是“研究者怎么得到这些知识”。

核心边界：

```text
Knowledge Store                Writer Context
---------------                --------------
Fact                     →     Fact
Conditions               →     Conditions
Exceptions               →     Exceptions
Required attribution     →     Required attribution
Internal source URL      ✕
Competitor domain        ✕
Research notes           ✕
YouTube timestamp        ✕
Evidence snippets        ✕
Investigation history    ✕
```

这是一层 **capability / context isolation**，不是简单禁词。

## 1. 输入契约

至少需要：

### A. Page Brief

```yaml
page_goal: "帮助玩家完成 Boss fight"
audience: "首次挑战该 Boss 的玩家"
content_type: "game-guide"
publication_mode: expert
```

### B. Canonical Facts

推荐来自 `fact-quality-control`，至少包含：

- statement；
- confidence；
- status；
- conditions / exceptions；
- publication_policy；
- attribution；
- provenance。

## 2. Publication Mode

### `expert`

默认隐藏 provenance 与来源身份。

适合：

- 游戏攻略；
- 软件教程；
- how-to；
- 工具使用说明；
- 普通知识型 utility 页面。

### `editorial`

只在来源身份对判断有帮助时保留 attribution。

适合：

- 产品评测；
- 旅游攻略；
- 商业分析；
- 行业文章。

### `evidence`

对关键结论保留必要 attribution。

适合：

- 新闻；
- 学术；
- 医疗；
- 法律；
- 金融；
- 政策；
- 统计与争议性事实。

若没有明确 mode：

- 普通教程/攻略默认 `expert`；
- 评测/分析默认 `editorial`；
- 高风险、新闻、学术、政策、争议事实默认 `evidence`。

不确定时宁可提高 attribution 暴露等级，不要隐藏本应公开的来源。

## 3. 输出契约

输出只包含 Writer 真正需要的信息：

```yaml
publication_context:
  publication_mode: expert
  page_goal: "..."
  audience: "..."
  facts:
    - statement: "Boss enters phase two at approximately 50% HP."
      conditions: []
      exceptions: []
      qualifier: null
      attribution: null
  uncertainties: []
  do_not_claim:
    - "Exact phase transition percentage beyond available evidence"
  editorial_constraints:
    - "Write from a knowledgeable player perspective"
    - "Do not narrate the research process"
```

默认 **不输出 provenance**。

## 4. 工作流

### Step 1 — 解析 Page Goal

确定：

- 用户完成页面后要做什么；
- 哪些问题必须回答；
- 哪些事实与任务无关；
- 是否需要版本、地区、平台限定。

### Step 2 — Filter by Publication Policy

- `ALLOW` → 可进入 context；
- `ALLOW_WITH_QUALIFIER` → 必须连 qualifier 一起进入；
- `REQUIRE_ATTRIBUTION` → 必须携带公开 attribution；
- `VERIFY_BEFORE_PUBLISH` → 默认排除，加入 `uncertainties / do_not_claim`；
- `DO_NOT_PUBLISH` → 排除。

### Step 3 — Relevance Selection

只选与当前页面任务有关的 facts。

不要因为知识库里有一条有趣事实，就把它塞进文章。

### Step 4 — Strip Internal Provenance

默认移除：

- URL；
- domain；
- 竞品名称；
- YouTube channel；
- Reddit username；
- timestamp；
- source title；
- evidence snippet；
- research notes；
- “我们搜索了什么”的过程信息。

例外：该事实为 `REQUIRE_ATTRIBUTION`，且发布策略允许公开来源。

### Step 5 — Build Writer-ready Facts

把 facts 按用户任务组织，而不是按来源组织。

坏：

```text
From YouTube:
- ...
From competitor A:
- ...
From Reddit:
- ...
```

好：

```text
Mechanics:
- ...
Route:
- ...
Failure cases:
- ...
```

### Step 6 — Create Negative Constraints

从被排除的冲突/低置信度事实生成 `do_not_claim`。

这样 Writer 不仅知道“能写什么”，还知道“不能擅自补什么”。

## 5. Attribution 处理

内部 provenance 与公开 attribution 是两个不同概念。

### 内部 provenance

用于：

- 审计；
- 重新验证；
- 追踪来源；
- 解决冲突。

默认不传给 Writer。

### 公开 attribution

只有在读者需要时传给 Writer。

例如：

```yaml
statement: "The agency revised the rule in 2026."
attribution:
  label: "Agency Name"
  required: true
```

不要把整个 research trail 一起暴露。

## 6. 不可违反的规则

1. **不能创造新事实。**
2. **不能把低置信度事实通过“更自然的措辞”洗成确定事实。**
3. **不能把内部 provenance 默认交给 Writer。**
4. **不能删除决定事实成立的 qualifier。**
5. **`REQUIRE_ATTRIBUTION` 不能被 expert mode 强行隐藏。**
6. **context 必须围绕页面任务，而不是围绕研究来源。**
7. **若 Writer 不需要某字段，就不要传。**

## 7. 典型转换

### Knowledge Store

```yaml
statement: "Boss enters phase two at approximately 50% HP."
confidence: high
provenance:
  - url: "https://competitor.example/..."
  - youtube: "video_01@03:52"
publication_policy:
  decision: ALLOW
attribution:
  mode: none
```

### Writer Context

```yaml
statement: "Boss enters phase two at approximately 50% HP."
```

Writer 从结构上就没有机会泄漏竞品 URL。

## 8. 交接到下游

下游通常是任意 Writer / LLM。

写完后推荐运行 `publication-qa`，检查是否仍出现：

- research meta language；
- source leakage；
- unsupported claims；
- certainty inflation。
