# Game Walkthrough Profile

用于 Level / Stage / Chapter / Puzzle 等需要**按顺序执行固定动作**的解法页面，尤其适合大量关卡型解谜游戏。

## 1. 核心原则

> Preserve exact state, action, order, and outcome. Never trade procedural accuracy for prose quality.

对 walkthrough 来说：

- 顺序是事实；
- 方向是事实；
- 次数是事实；
- 初始状态是事实；
- 中间状态可能也是事实；
- 最终结果是事实。

Writer 不得为了“读起来更自然”而重排步骤。

## 2. 页面目标

用户通常已经卡在具体关卡。页面首先要让用户能快速照着做并成功过关。

默认优先级：

1. Exact solution；
2. 必要的初始状态 / reset 提示；
3. 关键 failure point；
4. 必要机制解释；
5. 可验证的 alternative / shortcut。

原则：**solution first, explanation second**。

## 3. 推荐结构

```text
# Level / Stage / Puzzle [X] Solution

## Quick Solution
1. ...
2. ...
3. ...

## Reset / Starting State       (only if needed)
## Why This Works               (optional)
## Common Mistake               (optional)
## Alternative Solution         (optional)
## Version Note                 (optional)
```

不要为了模板完整而强行生成 optional 模块。

## 4. 程序正确性规则

### 4.1 不得重排步骤

如果输入是：

1. Move blue block left.
2. Rotate bridge.
3. Move character up.
4. Press switch.

输出必须保持同一语义顺序。

### 4.2 不得从结果反推路径

已知：

```text
The crate ends on the green switch.
```

不等于已知：

```text
Push left → go around → push up.
```

规则：

> known end state ≠ known solution path

若缺少中间动作，不得补造完整 solution。

### 4.3 不得混合多个方案

如果知识层提供：

```text
Solution A: 8 moves
Solution B: 6 moves
```

必须分别保留为完整方案，不能把两者步骤拼成一个不存在的新方案。

### 4.4 保留初始状态

如果解法依赖未操作过的棋盘/场景，应明确写：

```text
If you've already moved any pieces, restart the level first.
```

但只有输入事实支持时才能这样写。

### 4.5 保留动作精度

不得丢失或模糊：

- left / right / up / down；
- clockwise / counterclockwise；
- move count；
- tile / slot / room / platform；
- action target；
- action order；
- required timing（若有）。

## 5. Required Core + Optional Modules

### Required core

- level / puzzle identity；
- exact verified solution。

### Optional modules

只在上游有事实时加入：

- prerequisite；
- starting state；
- reset instruction；
- explanation；
- common mistake；
- alternative solution；
- shortest solution；
- shortcut；
- version note；
- reward / outcome。

不要让 AI 为每个关卡都硬编 Tips、Common Mistakes 或 Why This Works。

## 6. 不完整解法处理

如果上游信息只能确认部分步骤，应保持不完整状态，而不是猜测。

例如可在结构化输出中标记：

```yaml
unresolved_constraints:
  - "INCOMPLETE_SOLUTION: actions between step 3 and final state are not verified"
```

如果用户要求最终正文而完整解法又是页面核心，应明确说明当前上下文不足以安全生成完整步骤。

## 7. 文风

- 极具体；
- 极少废话；
- 动作优先；
- 一步一动作或一组不可拆动作；
- 用户应能边看边操作；
- 不加入“interesting puzzle”“clever mechanic”之类无助于过关的评价。

## 8. 与其他 Profile 的边界

- 游戏整体说明 → `game-overview`
- 机制/物品/FAQ → `game-reference`
- Boss / Quest / Unlock 等任务型攻略 → `game-guide`
- Level / Stage / Puzzle 的确定步骤 → `game-walkthrough`

## 9. 成功标准

真正的质量指标不是文章像不像“好文章”，而是：

```text
用户打开对应关卡
↓
几秒内找到解法
↓
能准确照着操作
↓
成功完成关卡
```
