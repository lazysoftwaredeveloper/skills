# Profile: Game Guide

## 目标

帮助玩家**完成具体游戏任务**，而不是向玩家展示作者做过多少研究。

适合：

- quest guide
- boss guide
- item/location guide
- walkthrough section
- mechanic explanation
- build/use guide

## 读者优先级

默认按以下顺序考虑信息：

1. 前置条件 / requirement
2. 去哪里 / location
3. 做什么 / action
4. 什么时候做 / timing or trigger
5. 会遇到什么 / mechanic or enemy
6. 怎么避免失败 / failure point
7. 完成后得到什么 / reward or outcome
8. 版本、平台或模式差异（如果 context 中存在）

不要为了“完整”擅自补齐缺失项目。

## 默认写法

优先：

- 具体动作；
- 方位和顺序；
- 条件触发；
- 可观察信号；
- 风险点贴近对应步骤；
- 关键限制提前说明。

坏：

```text
Players should be aware that this encounter can be challenging and requires careful preparation.
```

好（前提是 context 支持）：

```text
Save one defensive cooldown for the phase transition at roughly 50% HP.
```

## 语气

像熟悉游戏的玩家直接告诉另一个玩家怎么做：

- 不需要“根据攻略网站”；
- 不需要“多个玩家表示”；
- 不需要“我们的研究发现”；
- 不用夸张的专家人设；
- 不用空泛的“be prepared / stay alert”，除非后面跟具体做法。

## 信息结构

根据页面目标选择，不必机械套模板。

常见结构：

```text
Quick Answer
Requirements
Location / Route
Step-by-step
Mechanics / Timing
Common Failure Points
Rewards / What Happens Next
```

Boss 类常见结构：

```text
Before the Fight
Phase 1
Transition
Phase 2+
Dangerous Attacks
Positioning / Survival
Rewards
```

Quest 类常见结构：

```text
Requirements
Start Location
Objectives
Route / Steps
Items Needed
Turn-in / Completion
Rewards
```

## 特别禁止

不要自行补造：

- 地图坐标；
- 掉率；
- 精确伤害；
- 刷新概率；
- 等级要求；
- 版本变化；
- NPC 路线；
- “最优”策略。

除非这些内容明确存在于允许发布的 facts 中。

## 不确定信息

如果 context 给出 qualifier：

```text
approximately / may / can / typically / in some versions
```

必须保留其含义。

不要为了攻略“看起来更确定”而删除。
