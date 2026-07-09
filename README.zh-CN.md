# Maintainable Code Craft

[English](./README.md) | [简体中文](./README.zh-CN.md)

![Maintainable Code Craft](./assets/banner.svg)

[![License: MIT](https://img.shields.io/badge/License-MIT-1f2937.svg)](./LICENSE)
[![Codex Skill](https://img.shields.io/badge/Codex-Skill-0f766e.svg)](./agents/openai.yaml)
[![Focus: Maintainability](https://img.shields.io/badge/Focus-Maintainability-b45309.svg)](./SKILL.md)

写出三个月后再回头看，依然舒服、稳定、好维护的代码。

`maintainable-code-craft` 是一个可复用的 Codex skill，适合希望 AI 生成的代码更冷静、更清晰、更易审查，而不是过度炫技、过度抽象、噪音很重的团队和个人。

它会把 Codex 往这些方向拉：

- 更好的命名
- 更小的改动范围
- 更清楚的结构
- 更安全的配置方式
- 更明确的错误处理
- 更适合交给真人同事继续维护的代码

## 默认基线 Skill

对于“写代码 / 改代码”这类任务，这个 skill 的定位是默认基线 skill。

这意味着：

- 它是维护性、命名、结构、范围控制和工程品味的稳定基础层
- 它通常会和专项 skill 一起使用，而不是替代别的 skill
- 它天然不是排他性的
- 当 Codex 在写代码、改代码、重构、调试、审查代码时，它都适合作为常驻基础层存在

在实际执行里，`maintainable-code-craft` 往往会和这些 skill 协同：

- `tdd-workflow`：适合测试驱动的功能开发和 bug 修复
- `error-handling`：适合重试、降级、失败路径和错误设计
- `verification-loop`：适合最终验证和交付前检查
- 各类数据库专项 skill：适合 schema、query、migration、ORM 相关工作
- UI / design 类 skill：适合前端界面和体验优化任务

你可以把它理解成“维护性基础层”，而不是“任务专项替代品”。

## 为什么大家会用它

AI 可以把代码写得很快，但“写得快”不等于“以后好维护”。

这个 skill 适合你想让 Codex 从“追求新奇和花哨”切换到“追求工程品味”的场景：

- 用最简单且安全的设计解决真实问题
- 尽量保留项目本地约定，而不是一上来重写一大片
- 让副作用足够显眼
- 让公式、评分逻辑、数据处理链路都可审计
- 避免含糊命名、隐藏魔法和依赖膨胀

如果你想要的是“无聊，但无聊得很专业”的代码风格，这个 skill 很适合。

## 它会带来什么变化

它不是把 Codex 推向这种输出：

```text
Refactor this into a flexible manager with helper abstractions and utility layers.
```

而是更接近这种输出：

```text
Keep the change small. Use clear names. Make file writes explicit. Do not introduce new abstractions unless they remove real complexity.
```

通常结果会是：

- 代码更容易 review
- diff 更容易让人放心
- 更少无关重写
- 更少隐藏行为
- 更容易交接给下一个开发者

## 适用场景

这个 skill 很适合：

- 应用代码
- 脚本和自动化
- 测试
- 爬虫和 API client
- 数据管道
- 金融计算和评分逻辑
- UI 和 dashboard 代码
- 重构和代码审查

## 核心原则

- 命名表达意图，不用占位符式命名
- 函数聚焦单一责任
- 错误要可见、可定位、可处理
- 原始数据、标准化数据、派生结果尽量分层
- 优先遵守本地代码风格，而不是为了“通用最佳实践”制造 churn
- 只有在确实降低复杂度或风险时才新增依赖

## 快速开始

示例 prompt：

```text
Use maintainable-code-craft to refactor this module without changing behavior.
```

```text
Use maintainable-code-craft to review this patch for naming, structure, and maintainability issues.
```

```text
Use maintainable-code-craft while implementing this feature and keep the change small and readable.
```

```text
Use maintainable-code-craft to clean up this script and make error handling explicit.
```

## 安装

### 全局安装

把这个目录放到：

```text
~/.codex/skills/maintainable-code-craft/
```

### 项目级安装

把这个目录放到：

```text
<your-project>/.agents/skills/maintainable-code-craft/
```

## 仓库结构

```text
maintainable-code-craft/
  assets/
    banner.svg
  SKILL.md
  agents/
    openai.yaml
  README.md
  README.zh-CN.md
  LICENSE
```

## 包含文件

- `SKILL.md`：完整的维护性规则和工作方式说明
- `agents/openai.yaml`：Codex 侧的 skill 元数据
- `assets/banner.svg`：README 使用的仓库横幅图

## 说明

这个仓库只包含复用该 skill 所需的文件，不包含你的私有 Codex 配置、其他本地 skill、凭据或个人环境数据。

## License

MIT。见 [LICENSE](./LICENSE)。
