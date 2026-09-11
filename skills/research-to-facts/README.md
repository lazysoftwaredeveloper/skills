# Research to Facts Skill

把网页、视频、论坛、竞品、官方资料和研究笔记转换成结构化、可追溯的原子事实。

## Pipeline 位置

```text
Raw Research
    ↓
research-to-facts
    ↓
fact-quality-control
    ↓
Knowledge Store
```

## 核心原则

- 提取知识，不写文章；
- 事实与来源措辞解耦；
- provenance 保留在内部；
- 区分事实、观点、社区情绪、推断和估计；
- 不把 `according to...`、竞品 URL、研究过程带入事实正文。

## 使用示例

- `使用 research-to-facts 把这些竞品页面和 YouTube transcript 转成结构化事实`
- `从这些 Reddit 讨论中提取事实和 community sentiment，不要写文章`
- `把这批研究笔记标准化成下游 fact-quality-control 可以处理的格式`
