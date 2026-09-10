# Conversation Output Templates

这些模板用于 Conversation Mode。不要为了填满模板而制造不存在的数据。

## 1. SERP Step

### S{NNN} — SERP_EXPLORATION

| 字段 | 内容 |
|---|---|
| Parent Step(s) | |
| Trigger | |
| 目标 | |
| 假设 | |
| Query | `...` |
| Query Rationale | |
| Search interface | |
| Locale | |
| Discovery method | `ORGANIC_SERP` |

#### Organic / Returned Results

| Rank | Title（原文） | Domain | URL | Snippet / Description（原文） | Result Type | 分析 |
|---:|---|---|---|---|---|---|

#### Autocomplete
若不可用：`NOT_AVAILABLE_IN_CURRENT_INTERFACE`

| # | Suggestion（原文） | Dimension | 分析 |
|---:|---|---|---|

#### People Also Ask
若不可用：`NOT_AVAILABLE_IN_CURRENT_INTERFACE`

| # | Question（原文） | Potential Need / Pain | 分析 |
|---:|---|---|---|

#### Related Searches
若不可用：`NOT_AVAILABLE_IN_CURRENT_INTERFACE`

| # | Query（原文） | Dimension | 分析 |
|---:|---|---|---|

#### Findings
用中文描述这一步新增的知识，不重复粘贴全部表格。

#### Branch Decision

| Candidate | Source | Evidence | Status | Reason Code | 分析 / 原因 | Next / Revisit |
|---|---|---|---|---|---|---|

#### Next Action
明确写出 exact next query / page 与原因。

---

## 2. Site Exploration Step

### S{NNN} — SITE_EXPLORATION

| 字段 | 内容 |
|---|---|
| Site | |
| Parent Step(s) | |
| Trigger | |
| 目标 | |
| Discovery method | |

#### Coverage Audit

| Discovery Surface | Checked? | What Was Found | Coverage Notes |
|---|---|---|---|

Coverage: `HIGH / MEDIUM / LOW`

#### Page Inventory

| Page Title（原文） | URL | Page Type | Important Terms（原文） | 分析 | Follow-up? |
|---|---|---|---|---|---|

#### Vendor Phrase → Candidate Query

| Vendor Phrase（原文） | Candidate Query | 推导原因 | Status |
|---|---|---|---|

#### Findings / Decision / Next Action
保持 Evidence、分析、Decision 分离。

---

## 3. Page Review Step

### S{NNN} — PAGE_REVIEW

| 字段 | 内容 |
|---|---|
| URL | |
| Page title（原文） | |
| Site | |
| Discovery method | |
| Source Step | |
| 为什么打开 | |

#### Page Evidence

| Section / Location | Observed Feature / Claim（原文/事实） | Exact Phrase（原文） | Phrase Type | 抽象 Job | 分析 |
|---|---|---|---|---|---|

#### Useful Phrases

| Phrase（原文） | Type | Evidence Level | 分析 | Candidate Follow-up |
|---|---|---|---|---|

#### Findings / Decision / Next Action

---

## 4. Validation Step

### S{NNN} — VALIDATION

| 字段 | 内容 |
|---|---|
| Hypothesis under test | |
| Origin Step | |
| 为什么现在验证 | |
| Validation action | |

#### Evidence

| Source | Discovery method | Observed evidence（原文/事实） | Evidence Level | 分析 |
|---|---|---|---|---|

#### Verdict
`SUPPORTED / PARTIALLY_SUPPORTED / CONTRADICTED / INCONCLUSIVE`

说明候选状态如何变化。

---

## 5. Current Research State

| Root / Clue | First Seen | Evidence | Status | 最新判断 |
|---|---|---|---|---|

---

## 6. Discovery Graph

| From | Relation | To | Step | Evidence / Trigger | Status | 分析 |
|---|---|---|---|---|---|---|

---

## 7. Final Opportunity

### Opportunity: {name}

| 字段 | 内容 |
|---|---|
| Target user | |
| Job to Be Done | |
| Key pain | |
| Search roots / clusters | |
| Evidence trail | |
| Evidence levels | |
| Competition | |
| Paid-market signal | |
| Product concept | |
| Confidence | |

#### 为什么值得做
中文分析。

#### 为什么不是 Generic Product
指出它相对母词发生了哪些 Job / Vertical / Constraint 变化。

#### Open Questions
列出仍需 E5、更多 E4、技术验证等。

#### Rejected Alternatives
说明哪些相邻方向为什么被降级/淘汰。
