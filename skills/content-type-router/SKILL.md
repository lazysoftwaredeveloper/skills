---
name: content-type-router
description: 根据页面目标、用户意图、内容类型和任务结构，选择最合适的 writer profile，并输出 publication mode、required modules、optional modules 与 routing confidence。用于 Page Brief → Publication Context 之间的路由层。当前内置游戏站路由，并支持 software-tutorial 与 product-review。
compatibility: 适用于支持 Agent Skills 的 ChatGPT/Agent 环境。最佳输入是结构化 Page Brief；可独立运行，也可在 publication-context-builder 前运行。若页面类型无法确定，必须显式返回 ambiguous，不得猜测。
metadata:
  version: "1.0.0"
  language: "zh-CN"
  domain: "content-engineering"
---

# Content Type Router

## 0. 核心目标

这个 Skill 不写正文，也不判断事实真假。

它只回答：

> 这个页面的用户到底要完成什么，因此应该用哪一种写法？

推荐链路：

```text
Page Brief
    ↓
content-type-router
    ↓
publication-context-builder
    ↓
expert-content-writer + selected profile
```

## 1. 输入契约

推荐输入：

```yaml
page_brief:
  title: "Level 317 Solution"
  user_intent: "卡关，想知道这一关怎么过"
  page_type: "level_solution"
  entity_type: "game"
  audience: "正在玩 Level 317 的玩家"
  requested_profile: null
```

最少应提供以下一个或多个字段：

- title；
- user_intent；
- page_type；
- entity_type；
- page_goal；
- requested_profile。

## 2. 路由优先级

按以下顺序决策：

1. 用户明确指定 `requested_profile`；
2. 明确的 `page_type`；
3. 明确的 `user_intent / page_goal`；
4. title / slug 等弱信号。

禁止仅因为标题里出现某个词就覆盖更明确的用户任务。

## 3. 游戏内容路由表

### `game-overview`

适用：

- game overview；
- what is this game；
- how to play；
- beginner guide；
- core loop；
- game goal；
- world/chapter overview。

典型用户任务：

> “我第一次接触这个游戏，先告诉我它怎么玩。”

### `game-reference`

适用：

- mechanic；
- controls；
- item；
- character reference；
- rule；
- terminology；
- FAQ；
- system explanation。

典型用户任务：

> “这个东西是什么 / 怎么工作 / 有什么限制？”

### `game-guide`

适用：

- boss；
- quest；
- unlock；
- achievement；
- collectible；
- route；
- build；
- strategy；
- 完成一个明确任务但不要求唯一固定动作序列。

典型用户任务：

> “我怎样完成这个目标？”

### `game-walkthrough`

适用：

- level solution；
- stage solution；
- puzzle solution；
- chapter walkthrough；
- 需要保留固定顺序、方向、次数或状态变化的程序型解法。

典型用户任务：

> “我卡在这一关，告诉我准确按什么顺序操作。”

## 4. 非游戏基础映射

### `software-tutorial`

适用：

- software how-to；
- setup；
- configuration；
- command / UI workflow；
- troubleshooting procedure。

### `product-review`

适用：

- product review；
- hands-on evaluation；
- pros / cons；
- fit / not fit；
- buying-oriented comparison。

若没有匹配 profile，不得伪造 profile 名；应返回 `generic` 或 `ambiguous`。

## 5. 输出契约

推荐输出：

```yaml
routing_decision:
  status: resolved
  writer_profile: game-walkthrough
  publication_mode: expert
  routing_confidence: high
  reason: "页面目标是给出 Level 317 的精确过关步骤，顺序本身属于事实。"
  required_modules:
    - exact_solution
  optional_modules:
    - starting_state
    - reset_instruction
    - common_mistake
    - alternative_solution
  context_priorities:
    - ordered_steps
    - action_target
    - direction
    - count
    - outcome
```

## 6. Publication Mode 默认值

默认映射：

- `game-overview` → `expert`
- `game-reference` → `expert`
- `game-guide` → `expert`
- `game-walkthrough` → `expert`
- `software-tutorial` → `expert`
- `product-review` → `editorial`

如果上游明确要求 evidence mode 或 required attribution，则不得降级。

## 7. 游戏站常见组合

一个完整的关卡型游戏站通常不是一个 profile，而是多个 profile 共用同一 Knowledge Store：

```text
Homepage / How to Play       → game-overview
Controls / Mechanics / FAQ   → game-reference
Boss / Unlock / Collectible  → game-guide
Level 1..N / Puzzle          → game-walkthrough
```

同一事实可以被多个页面使用，但表达方式不同。

## 8. 歧义处理

若无法可靠判断，例如：

```text
"World 5"
```

它可能是：

- world overview；
- world walkthrough；
- unlock guide。

则输出：

```yaml
routing_decision:
  status: ambiguous
  candidates:
    - game-overview
    - game-guide
    - game-walkthrough
  missing_signal:
    - "用户是想了解 World 5、解锁 World 5，还是逐关通关？"
```

若当前工作流不允许追问，则选最保守的 generic 路径，并明确 `routing_confidence: low`；不要把推测伪装成确定路由。

## 9. 不可违反的规则

1. **不写正文。**
2. **不研究事实。**
3. **不改变事实 confidence。**
4. **不因为 SEO 标题词覆盖真实用户任务。**
5. **不把所有游戏页都路由到 game-guide。**
6. **程序型关卡解法优先识别为 game-walkthrough。**
7. **机制/FAQ 优先识别为 game-reference。**
8. **页面整体认知优先识别为 game-overview。**
9. **无法判断时显式保留歧义。**
