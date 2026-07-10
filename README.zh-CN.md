[English](./README.md) | [简体中文](./README.zh-CN.md)

![Maintainable Code Craft](./assets/banner.svg)

[![License: MIT](https://img.shields.io/badge/license-MIT-1f2937.svg)](./LICENSE)
[![Codex Skill](https://img.shields.io/badge/Codex-skill-0f766e.svg)](./SKILL.md)

让代码过三个月再回头改，依然清楚、顺手。

`maintainable-code-craft` 是一个可复用的 Codex skill。它给写代码和改代码的任务提供一套稳定的维护性基线，让改动更克制、更容易审查，同时把具体任务的专业流程留给对应的专项 skill。

> 基线保持稳定，专项能力随任务切换。

## 它是基础层，不是整套工具箱

对于写代码、改代码这类任务，这个 skill 负责守住几件基础但重要的事：

- 命名清楚，代码容易读
- 改动范围可控，尊重项目原有习惯
- 结构不过度，副作用看得见
- 错误明确，配置安全
- 不随意增加依赖，交付前做必要验证

它不排斥其他 skill。真正执行任务时，通常把它和专项 skill 一起使用。

```text
             tdd-workflow      error-handling      interface-design
                    \                |                /
                     \             专项层             /
                      +------------------------------+
                      |  maintainable-code-craft     |
                      |       稳定的维护性基础层       |
                      +------------------------------+
```

## 如何组合使用

| 任务 | 建议组合 |
|---|---|
| 开发功能或修复 bug | `maintainable-code-craft` + `tdd-workflow` |
| 设计失败路径与降级逻辑 | `maintainable-code-craft` + `error-handling` |
| 完成一次重要改动 | `maintainable-code-craft` + `verification-loop` |
| 修改数据库结构或查询 | `maintainable-code-craft` + 数据库专项 skill |
| 开发正式的前端界面 | `maintainable-code-craft` + UI/design skill |

专项 skill 负责更深入的工作流程，`maintainable-code-craft` 负责让最终代码清楚、克制、方便维护。

## 实际会有什么不同

缺少维护性约束时，AI 编码很容易把一个局部任务扩大成大范围重写，出现含糊命名、多余抽象或隐藏副作用。

使用这个 skill 后，Codex 会被要求：

- 动手前先了解现有项目
- 只做解决问题所需的最小安全改动
- 除非原有约定确实有害，否则继续沿用
- 让错误、配置和副作用保持可见
- 只有在确实降低复杂度时才增加抽象或依赖
- 按改动风险选择合适的验证方式

### 示例

不要只给一个范围很大的要求：

```text
Refactor this module into a flexible architecture with reusable managers and utilities.
```

可以把边界说得更清楚：

```text
Use $maintainable-code-craft to refactor this module without changing behavior.
Keep the diff small, preserve local conventions, and make file writes explicit.
```

## 核心原则

- 命名表达真实意图，不用占位符凑合
- 函数职责集中，副作用一眼能看出来
- 优先保持项目内部一致，不为了套“最佳实践”制造无关改动
- 不用空 catch 或假成功掩盖失败
- 不把密钥和环境相关配置写进代码
- 重要行为和边界情况尽量有测试
- 问题已经解决清楚时，不再继续增加结构

数据处理和金融计算的补充规则放在按需参考文件中，不会跟着每一个普通编码任务一起加载。

## 安装

### 全局安装

如果希望所有项目都能使用这个 skill，可以安装到全局目录。

macOS 或 Linux：

```bash
skills_root="${CODEX_HOME:-$HOME/.codex}/skills"
mkdir -p "$skills_root"
git clone https://github.com/Patrickpoix/maintainable-code-craft.git \
  "$skills_root/maintainable-code-craft"
```

Windows PowerShell：

```powershell
$skillsRoot = if ($env:CODEX_HOME) {
    Join-Path $env:CODEX_HOME "skills"
} else {
    Join-Path $HOME ".codex\skills"
}

New-Item -ItemType Directory -Force $skillsRoot | Out-Null
git clone https://github.com/Patrickpoix/maintainable-code-craft.git `
    (Join-Path $skillsRoot "maintainable-code-craft")
```

### 项目级安装

如果希望仓库里的每位协作者都能使用，建议通过 Git submodule 安装。这样不会在项目里藏一个无法正常跟踪的嵌套 Git 仓库。

macOS 或 Linux：

```bash
mkdir -p .agents/skills
git submodule add https://github.com/Patrickpoix/maintainable-code-craft.git \
  .agents/skills/maintainable-code-craft
```

Windows PowerShell：

```powershell
New-Item -ItemType Directory -Force ".agents\skills" | Out-Null
git submodule add https://github.com/Patrickpoix/maintainable-code-craft.git `
    ".agents\skills\maintainable-code-craft"
```

克隆一个已经包含该 skill 的项目后，运行下面的命令完成初始化：

```bash
git submodule update --init --recursive
```

更新全局安装：

```bash
git -C <全局安装路径> pull --ff-only
```

更新项目里的 submodule：

```bash
git submodule update --remote .agents/skills/maintainable-code-craft
```

安装完成后重启 Codex，让它重新发现可用的 skills。

## 检查是否安装成功

新建一个 Codex 任务，显式调用这个 skill：

```text
Use $maintainable-code-craft to review this module for naming, scope, and hidden side effects.
```

它的描述已经覆盖常见编码任务，因此 Codex 可以自动选择它；如果希望确保本次任务一定使用，直接写出 `$maintainable-code-craft` 最稳妥。

## 仓库结构

```text
maintainable-code-craft/
├── SKILL.md
├── agents/
│   └── openai.yaml
├── references/
│   └── data-and-financial.md
├── assets/
│   └── banner.svg
├── README.md
├── README.zh-CN.md
└── LICENSE
```

- `SKILL.md`：精简后的默认基线规则
- `references/`：只在对应任务中加载的专项说明
- `agents/openai.yaml`：Codex 使用的界面元数据
- `assets/banner.svg`：README 顶部横幅

## 参与改进

欢迎提交 Issue 或 Pull Request。新增规则应尽量适用于不同项目，篇幅要适合反复加载，并且能和专项 skill 一起工作。

## License

本项目采用 [MIT License](./LICENSE)。
