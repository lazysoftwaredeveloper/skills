# Game Overview Profile

用于游戏首页、How to Play、Beginner Guide、玩法概览、世界/章节概览等“先帮助读者理解游戏”的页面。

## 1. 页面目标

让一个对游戏了解很少的读者快速建立正确心智模型：

- 这是什么游戏；
- 玩家最终要做什么；
- 一次典型游玩循环是什么；
- 核心机制有哪些；
- 关卡/世界/章节如何推进；
- 新手开始前最需要知道什么。

重点是 **mental model first**，不是逐关给答案。

## 2. 信息优先级

优先顺序通常为：

1. 一句话说明游戏与核心目标；
2. Core loop / main goal；
3. 核心规则与机制；
4. 基础操作或交互；
5. Progression / levels / worlds；
6. 真正影响开局体验的 beginner tips；
7. 指向更具体 guide / reference / walkthrough 的导航信息。

只有输入事实支持时才写对应模块。

## 3. 推荐结构

```text
What Is [Game]?
How the Game Works
Main Goal
Core Mechanics
Controls / Basic Interaction   (optional)
Levels and Progression         (optional)
Beginner Tips                  (optional)
Where to Go Next               (optional)
```

不要为了填模板而制造空章节。

## 4. 写作规则

- 先解释“怎么玩”，再补背景。
- 用玩家能执行和理解的语言解释机制，不用营销文案。
- 不把关卡答案大段塞进 overview。
- 可以举例说明机制，但例子必须来自允许发布的事实。
- 不因为“overview 应该完整”而补造未验证的游戏规则。
- 对版本、平台、模式差异保留必要 qualifier。

## 5. 与其他游戏 Profile 的边界

### 使用 `game-overview`

- 游戏介绍；
- How to Play；
- Beginner Guide；
- 游戏目标与核心循环；
- World / chapter 的总体介绍。

### 改用 `game-reference`

当页面主要回答一个具体机制、物品、角色、术语或 FAQ。

### 改用 `game-guide`

当用户有一个明确任务，如解锁、Boss、Quest、Achievement、Collectible。

### 改用 `game-walkthrough`

当用户需要固定、按顺序执行的关卡/谜题解法。

## 6. 禁止事项

- 不写“我们的研究发现”；
- 不用泛泛背景撑字数；
- 不把猜测写成玩法规则；
- 不在 overview 中复制大量 level-by-level solution；
- 不制造不存在的 progression、难度、奖励或机制。

## 7. 成功标准

读者看完后，应能回答：

> “这个游戏到底要我做什么，我主要怎么做，以及接下来应该去哪里找更具体的帮助？”
