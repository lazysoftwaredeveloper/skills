# Profile: Software Tutorial

## 目标

帮助用户**成功完成一个软件操作或技术任务**，并能判断自己是否做对。

适合：

- GUI 操作教程
- CLI / developer workflow
- configuration guide
- setup / installation
- troubleshooting article
- feature how-to

## 读者优先级

优先表达：

1. 前置条件；
2. 准确入口或命令；
3. 操作步骤；
4. 参数 / 选项含义；
5. 成功时应看到什么；
6. 常见失败点；
7. 版本 / OS / 权限差异（若 context 中存在）。

## 默认结构

```text
What You’ll Do / Quick Answer
Prerequisites
Steps
Expected Result
Troubleshooting
Version / Platform Notes
```

只保留有内容支持的部分。

## 写法

优先使用可执行表达：

```text
Open Settings → Privacy → ...
Run `command --flag value`.
After saving, the status should change to ...
```

不要写：

```text
Research indicates that users can configure this option through Settings.
```

应直接写：

```text
Open Settings and select ...
```

## 命令与代码

- 不得发明参数；
- 不得猜测路径；
- 不得把不同版本的命令混成一个；
- 必须保留 context 中的 shell / OS / version 条件；
- 若命令结果未知，不要虚构 expected output。

## Troubleshooting

只有当 context 中存在失败条件、错误信息或恢复步骤时才写。

不要为了显得完整而生成通用故障排除，例如：

```text
Restart your computer.
Check your internet connection.
Reinstall the application.
```

除非事实支持。

## 语气

- 精确；
- 简洁；
- 操作导向；
- 不假装亲自测试过，除非输入明确说明；
- 不使用“we tested / we verified”来制造第一手经验。
