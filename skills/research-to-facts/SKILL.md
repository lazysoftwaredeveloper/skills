---
name: research-to-facts
description: 将网页、YouTube、论坛、竞品、官方文档、访谈、研究笔记等杂乱研究材料，转换为可复用、可追溯、与来源措辞解耦的原子事实。适用于任何“先研究、后写作”的内容工作流，是 Research → Knowledge 的第一层。
compatibility: 适用于支持 Agent Skills 的 ChatGPT/Agent 环境。可处理用户提供材料，也可接收其他研究 Skill 的输出。若缺少足够证据，必须保留不确定性，禁止补造事实。
metadata:
  version: "1.0.0"
  language: "zh-CN"
  domain: "content-engineering"
---

# Research to Facts

## 0. 指令优先级

用户当前明确指令优先于本 Skill。若用户要求只提取某类事实、保留特定引用、忽略某些来源或改变输出格式，应服从用户，但不得把推断伪装成事实。

## 1. 核心目标

把“来源说了什么”转换成“我们实际知道什么”，同时把 provenance 保留在内部字段中。

核心链路：

`raw sources → evidence snippets → atomic claims → normalized facts + provenance`

本 Skill **不负责写最终文章**。

## 2. 何时使用

适合：

- 研究多个竞品页面后提取可复用知识；
- 从 YouTube transcript / timestamp 提取机制、步骤、行为；
- 从 Reddit / 论坛区分事实与社区观点；
- 从官方文档、发布说明、规格页提取规则与参数；
- 将杂乱 research notes 变成下游 Writer 可消费的知识；
- 为后续 `fact-quality-control` 建立统一输入。

不适合：

- 直接写 SEO 文章、攻略、教程；
- 直接决定一个争议事实是否“已验证”；
- 为了让文本好看而补齐来源没有提供的信息。

## 3. 输入契约

输入可以是任意组合：

- 原文片段；
- URL / 页面标题 / source id；
- 视频 transcript / timestamp；
- 用户评论；
- 表格；
- 研究笔记；
- 其他 Agent 输出。

若来源身份可知，为每个来源分配稳定 `source_id`。

建议记录：

```yaml
source_id: source_01
source_type: official | documentation | competitor | youtube | forum | social | user_provided | other
origin: "原始 URL、文件名或可识别来源"
published_at: null
retrieved_at: null
```

## 4. 输出契约

默认输出结构化事实列表：

```yaml
facts:
  - fact_id: fact_001
    statement: "Boss enters phase two at approximately 50% HP."
    claim_type: FACT
    subject: "Boss"
    scope: "phase transition"
    conditions: []
    exceptions: []
    temporal_scope: null
    confidence_hint: high
    provenance:
      - source_id: source_01
        evidence: "来源中真正支持该事实的最小必要片段或观察说明"
        locator: "page section / timestamp / line / note id"
```

`statement` 必须能够在脱离来源名称后独立成立。

## 5. Claim Type

只能从以下类型中选择最合适的一种：

- `FACT`：来源直接陈述或可直接观察的客观命题；
- `OBSERVATION`：研究者从材料中直接观察到的行为或现象；
- `OPINION`：某个明确主体的主观看法；
- `COMMUNITY_SENTIMENT`：社区中重复出现的主观倾向；
- `INFERENCE`：由多个事实推导出的解释；
- `ESTIMATE`：近似值、范围或不精确数值；
- `UNVERIFIED_CLAIM`：来源提出但当前无法独立确认的说法。

不得把 `OPINION / SENTIMENT / INFERENCE` 升级成 `FACT`。

## 6. 工作流

### Step 1 — 建立 Source Inventory

先识别材料来自哪里。来源信息只用于 provenance，不要因为来源名气大就自动判定事实为真。

### Step 2 — 提取最小 Evidence

对每个潜在事实，保留真正支持它的最小证据片段、timestamp、表格项或观察说明。

禁止把整篇文章复制成 evidence。

### Step 3 — 原子化 Claim

一个事实只表达一个主要命题。

坏：

> The boss enters phase two at 50% HP, gains an AoE, and becomes much harder.

好：

- Boss enters phase two at approximately 50% HP.
- Phase two adds a circular AoE attack.
- The fight becomes harder. → 如果只是主观看法，应标 `OPINION` 或删除。

### Step 4 — Source-independent Normalization

把来源依赖型表达改造成独立知识。

坏：

> According to IGN, the weapon unlocks at level 15.

好：

> The weapon unlocks at level 15.

`IGN` 只保留在 provenance。

### Step 5 — 补齐 Qualifier

检查是否存在：

- 版本；
- 地区；
- 平台；
- 游戏模式；
- 前置条件；
- 时间范围；
- 例外；
- 近似值。

不要为了“简洁”删除决定事实真假的限定条件。

### Step 6 — 初步 Confidence Hint

仅给下游质量控制提供提示：

- `high`：直接、明确、证据贴合；
- `medium`：有支持但存在范围、解释或单一来源依赖；
- `low`：含冲突、弱证据、间接推断或无法确认。

最终置信度由 `fact-quality-control` 决定。

## 7. 不可违反的规则

1. **不要写最终文章。**
2. **不要把来源名称、URL、YouTube 频道、竞品品牌写进 `statement`，除非“谁说了什么”本身就是待记录事实。**
3. **不要把 research process 写进 `statement`。**
4. **不要补造缺失数字、步骤、条件、版本或因果关系。**
5. **不要把多个来源重复同一句话误认为多个独立事实。**
6. **保留 provenance，但让事实本身与 provenance 解耦。**
7. **原文不确定时，事实也必须保留不确定性。**

## 8. 示例

### 示例 A — 多来源事实

输入：

- Competitor A: “The key is on the desk in Room 203.”
- YouTube 04:32: 玩家从 Room 203 的桌面拾取该 key。

输出：

```yaml
- fact_id: fact_001
  statement: "The key can be found on the desk in Room 203."
  claim_type: FACT
  confidence_hint: high
  provenance:
    - source_id: competitor_a
      locator: "guide section"
    - source_id: youtube_01
      locator: "04:32"
```

### 示例 B — 社区观点

输入：

> Reddit 多个评论称这个 Boss “annoying / frustrating”。

输出：

```yaml
- fact_id: fact_002
  statement: "Many community comments describe the boss as frustrating."
  claim_type: COMMUNITY_SENTIMENT
  confidence_hint: medium
```

不得输出：

> The boss is badly designed.

## 9. 交接到下游

推荐下一步：`fact-quality-control`。

传递：

- normalized facts；
- provenance；
- qualifiers；
- confidence hints。

不要在这一层删除 provenance；真正的发布隔离发生在 `publication-context-builder`。
