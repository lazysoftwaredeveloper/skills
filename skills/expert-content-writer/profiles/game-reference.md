# Game Reference Profile

用于规则、机制、物品、角色、术语、Controls、FAQ、系统说明等“查一个明确知识点”的页面。

## 1. 页面目标

帮助读者快速、准确地回答一个具体问题，而不是带读者走完整关卡流程。

典型问题：

- How does the red switch work?
- What does this item do?
- How do checkpoints work?
- What are the controls?
- How many worlds are there?

## 2. 信息优先级

通常按以下顺序组织：

1. 直接定义 / 直接答案；
2. How it works；
3. Conditions / limitations / exceptions；
4. Where / when it appears（若已验证）；
5. Related mechanics / interactions（若与用户理解直接相关）；
6. FAQ / edge cases（仅在有事实支持时）。

## 3. 推荐结构

```text
[Direct answer / Definition]
How It Works
Conditions and Limitations   (optional)
Where It Appears             (optional)
Related Mechanics            (optional)
FAQ                          (optional)
```

简单问题可以只用一两段，不要机械扩成完整模板。

## 4. 写作规则

- 第一段尽快给答案，不用长背景铺垫。
- 区分规则、条件、例外和玩家策略。
- 不把“常见做法”写成系统硬规则。
- 不从某个关卡里的表现反推出全局机制，除非知识层明确支持。
- 术语保持一致；同一对象不要随意换称呼。
- 数字、范围、版本、平台差异必须保持 qualifier。

## 5. 与其他游戏 Profile 的边界

- 游戏整体玩法与新手理解 → `game-overview`
- 某个 Boss / Quest / Unlock / Achievement 的完成方法 → `game-guide`
- 固定步骤的关卡/谜题答案 → `game-walkthrough`
- 单个机制、物品、术语、FAQ → `game-reference`

## 6. 禁止事项

- 不把 reference 写成长篇 walkthrough；
- 不凭上下文补全未验证规则；
- 不使用“据玩家反馈”替代事实分类；
- 不泄漏内部来源或研究过程；
- 不为了“完整”生成不存在的 edge cases。

## 7. 成功标准

读者应能在很短时间内知道：

> “它是什么、怎么工作、什么时候不适用。”
