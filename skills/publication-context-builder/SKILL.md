---
name: publication-context-builder
description: 从经过质量控制的知识库中，为具体文章、攻略、教程或页面构造“发布上下文”：只选择与任务相关且允许发布的事实，并默认移除内部 provenance、竞品 URL、研究笔记和证据噪音。可消费 content-type-router 的 routing_decision，把 writer profile、required modules 与 context priorities 一并传给 Writer。
compatibility: 适用于支持 Agent Skills 的 ChatGPT/Agent 环境。最佳输入来自 fact-quality-control + Page Brief；可选接收 content-type-router 输出。可用于游戏、软件、SEO、旅游、产品评测、研究型内容等领域，并通过 publication mode 控制 attribution 暴露。
metadata:
  version: "1.1.0"
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
Sequence / state          →     Sequence / state when semantically required
Internal source URL      ✕
Competitor domain        ✕
Research notes           ✕
YouTube timestamp        ✕
Evidence snippets        ✕
Investigation history    ✕
```

这是一层 **capability / context isolation**，不是简单禁词。

## 1. 输入契约

### A. Page Brief

```yaml
page_goal: "帮助玩家完成 Level 317"
audience: "正在玩 Level 317 的玩家"
content_type: "level_solution"
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

对于程序型内容，还可包含：

- sequence；
- starting_state；
- resulting_state；
- action_target；
- direction；
- count；
- solution_id。

### C. Routing Decision（可选但推荐）

来自 `content-type-router`：

```yaml
routing_decision:
  writer_profile: game-walkthrough
  publication_mode: expert
  required_modules:
    - exact_solution
  optional_modules:
    - starting_state
    - common_mistake
  context_priorities:
    - ordered_steps
    - direction
    - count
    - outcome
```

如果 Router 没有提供，则沿用 Page Brief / 调用方指定的 content type 与 publication mode。

## 2. Publication Mode

### `expert`

默认隐藏 provenance 与来源身份。

适合：游戏攻略、游戏 reference、walkthrough、软件教程、how-to、工具使用说明。

### `editorial`

只在来源身份对判断有帮助时保留 attribution。

适合：产品评测、旅游攻略、商业分析、行业文章。

### `evidence`

对关键结论保留必要 attribution。

适合：新闻、学术、医疗、法律、金融、政策、统计与争议性事实。

若没有明确 mode：

- 普通教程/攻略默认 `expert`；
- 评测/分析默认 `editorial`；
- 高风险、新闻、学术、政策、争议事实默认 `evidence`。

不确定时宁可提高 attribution 暴露等级，不要隐藏本应公开的来源。

## 3. 输出契约

```yaml
publication_context:
  publication_mode: expert
  page_goal: "帮助玩家完成 Level 317"
  audience: "正在玩 Level 317 的玩家"
  content_type: "level_solution"
  writer_profile: game-walkthrough
  required_modules:
    - exact_solution
  optional_modules:
    - starting_state
    - common_mistake
  facts:
    - statement: "Move the blue block left twice."
      sequence: 1
      direction: left
      count: 2
    - statement: "Rotate the center platform clockwise."
      sequence: 2
  uncertainties: []
  do_not_claim:
    - "Any unverified steps between the observed actions"
  editorial_constraints:
    - "Preserve procedural order exactly"
```

默认 **不输出 provenance**。

## 4. 工作流

### Step 1 — 解析 Page Goal 与 Routing Decision

确定：

- 用户完成页面后要做什么；
- writer profile；
- required / optional modules；
- context priorities；
- 哪些问题必须回答；
- 哪些事实与任务无关；
- 是否需要版本、地区、平台限定。

Router 决定“写哪一种页面”，Context Builder 决定“这页允许 Writer 看到哪些知识”。

### Step 2 — Filter by Publication Policy

- `ALLOW` → 可进入 context；
- `ALLOW_WITH_QUALIFIER` → 必须连 qualifier 一起进入；
- `REQUIRE_ATTRIBUTION` → 必须携带公开 attribution；
- `VERIFY_BEFORE_PUBLISH` → 默认排除，加入 `uncertainties / do_not_claim`；
- `DO_NOT_PUBLISH` → 排除。

### Step 3 — Relevance Selection

只选与当前页面任务有关的 facts。

不要因为知识库里有一条有趣事实，就把它塞进文章。

### Step 4 — Preserve Semantic Structure

如果 routing decision 表明页面是程序型内容（如 `game-walkthrough`），不得把有序结构压平成无序事实集合。

必须保留所有影响操作正确性的字段，例如：

- sequence；
- starting_state；
- resulting_state；
- direction；
- count；
- action_target；
- solution_id。

规则：

> Strip provenance, not procedure semantics.

### Step 5 — Strip Internal Provenance

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

### Step 6 — Build Writer-ready Facts

把 facts 按用户任务组织，而不是按来源组织。

坏：

```text
From YouTube:
- ...
From competitor A:
- ...
```

好：

```text
Starting state:
- ...
Ordered solution:
1. ...
2. ...
Outcome:
- ...
```

或：

```text
Mechanics:
- ...
Conditions:
- ...
Exceptions:
- ...
```

### Step 7 — Required / Optional Module Gate

- required module：必须有足够事实才能交给 Writer 成稿；
- optional module：只有有事实支持时才传给 Writer；
- 不得因为模板要求而补造 Tips、FAQ、Common Mistakes、Alternative Solution。

若 `exact_solution` 是 required 但事实不完整，应生成 uncertainty / do_not_claim，而不是补齐步骤。

### Step 8 — Create Negative Constraints

从被排除的冲突、低置信度、缺失步骤中生成 `do_not_claim`。

这样 Writer 不仅知道“能写什么”，还知道“不能擅自补什么”。

## 5. Attribution 处理

内部 provenance 与公开 attribution 是两个不同概念。

### 内部 provenance

用于审计、重新验证、追踪来源、解决冲突，默认不传给 Writer。

### 公开 attribution

只有在读者需要时传给 Writer。

```yaml
statement: "The agency revised the rule in 2026."
attribution:
  label: "Agency Name"
  required: true
```

不要把整个 research trail 一起暴露。

## 6. 不可违反的规则

1. **不能创造新事实。**
2. **不能把低置信度事实通过更自然的措辞洗成确定事实。**
3. **不能把内部 provenance 默认交给 Writer。**
4. **不能删除决定事实成立的 qualifier。**
5. **`REQUIRE_ATTRIBUTION` 不能被 expert mode 强行隐藏。**
6. **context 必须围绕页面任务，而不是围绕研究来源。**
7. **若 Writer 不需要某字段，就不要传。**
8. **程序型内容必须保留 sequence / state / direction / count 等语义。**
9. **required module 信息不完整时，不得靠推断补齐。**
10. **optional module 无事实支持时必须省略。**

## 7. 典型转换

### Knowledge Store

```yaml
statement: "Move the blue block left twice."
sequence: 1
direction: left
count: 2
provenance:
  - youtube: "video_01@03:52"
publication_policy:
  decision: ALLOW
```

### Writer Context

```yaml
statement: "Move the blue block left twice."
sequence: 1
direction: left
count: 2
```

Writer 从结构上没有机会泄漏 YouTube，但仍保留了过关所需的操作语义。

## 8. 交接到下游

推荐交给 `expert-content-writer`，由 `writer_profile` 选择具体表达策略；写完后运行 `publication-qa`。
