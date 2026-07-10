[English](./README.md) | [简体中文](./README.zh-CN.md)

![Maintainable Code Craft](./assets/banner.svg)

[![License: MIT](https://img.shields.io/badge/license-MIT-1f2937.svg)](./LICENSE)
[![Codex Skill](https://img.shields.io/badge/Codex-skill-0f766e.svg)](./SKILL.md)

Write code that still feels clear when someone has to change it three months later.

`maintainable-code-craft` is a reusable Codex skill for calmer, smaller, and more reviewable code changes. It gives Codex a consistent maintainability baseline without taking over the specialist workflow needed for the task.

> The baseline stays constant. The specialist changes with the job.

## A Baseline, Not The Whole Toolbox

For code-writing and code-changing work, this skill is designed to be the default foundation for:

- Naming and readability
- Change scope and local consistency
- Structure and visible side effects
- Explicit errors and safe configuration
- Dependency restraint and practical verification

It is deliberately non-exclusive. Use it with specialist skills rather than instead of them.

```text
             tdd-workflow      error-handling      interface-design
                    \                |                /
                     \        task-specific layer    /
                      +------------------------------+
                      |  maintainable-code-craft     |
                      |  stable maintainability base |
                      +------------------------------+
```

## How It Composes

| Task | Suggested skill combination |
|---|---|
| Build or fix a feature | `maintainable-code-craft` + `tdd-workflow` |
| Design failure paths | `maintainable-code-craft` + `error-handling` |
| Finish a meaningful change | `maintainable-code-craft` + `verification-loop` |
| Change schemas or queries | `maintainable-code-craft` + a database-specific skill |
| Build a polished interface | `maintainable-code-craft` + a UI/design skill |

The specialist skill owns the deeper workflow. `maintainable-code-craft` keeps the result readable, scoped, and maintainable.

## What Changes In Practice

Without a maintainability baseline, an AI coding task can drift toward broad rewrites, vague names, unnecessary abstractions, or hidden side effects.

With this skill, Codex is instructed to:

- Inspect the existing project before editing
- Make the smallest safe change that solves the request
- Preserve local conventions unless they are harmful
- Make errors, configuration, and side effects visible
- Add abstractions and dependencies only when they remove real complexity
- Verify changed behavior at a level proportional to risk

### Example

Instead of a broad instruction:

```text
Refactor this module into a flexible architecture with reusable managers and utilities.
```

Use a scoped instruction:

```text
Use $maintainable-code-craft to refactor this module without changing behavior.
Keep the diff small, preserve local conventions, and make file writes explicit.
```

## Core Principles

- Name things by intent, not by placeholder
- Keep functions focused and side effects obvious
- Prefer local consistency over generic best-practice churn
- Never hide failures behind fake success or empty catches
- Keep secrets and environment-specific values out of source code
- Test important behavior and edge cases when feasible
- Stop adding structure when the problem is already solved clearly

Data-heavy and financial tasks receive additional guidance through an on-demand reference instead of loading those rules for every coding task.

## Install

### Global installation

Install globally when you want the skill available across projects.

macOS or Linux:

```bash
skills_root="${CODEX_HOME:-$HOME/.codex}/skills"
mkdir -p "$skills_root"
git clone https://github.com/Patrickpoix/maintainable-code-craft.git \
  "$skills_root/maintainable-code-craft"
```

Windows PowerShell:

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

### Project-local installation

Use a Git submodule when a repository should carry the skill for everyone working in that project. This avoids hiding a nested Git repository inside the project.

macOS or Linux:

```bash
mkdir -p .agents/skills
git submodule add https://github.com/Patrickpoix/maintainable-code-craft.git \
  .agents/skills/maintainable-code-craft
```

Windows PowerShell:

```powershell
New-Item -ItemType Directory -Force ".agents\skills" | Out-Null
git submodule add https://github.com/Patrickpoix/maintainable-code-craft.git `
    ".agents\skills\maintainable-code-craft"
```

After cloning a project that already includes the skill, initialize it with:

```bash
git submodule update --init --recursive
```

Update a global installation with:

```bash
git -C <global-installation-path> pull --ff-only
```

Update the project submodule with:

```bash
git submodule update --remote .agents/skills/maintainable-code-craft
```

Restart Codex after installation so it can rediscover available skills.

## Verify The Installation

Start a new Codex task and invoke the skill explicitly:

```text
Use $maintainable-code-craft to review this module for naming, scope, and hidden side effects.
```

The skill is designed to match coding tasks automatically, but explicit invocation is useful when you want to guarantee that it is included.

## Repository Layout

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

- `SKILL.md` contains the compact default baseline.
- `references/` contains guidance loaded only for matching domains.
- `agents/openai.yaml` contains Codex-facing interface metadata.
- `assets/banner.svg` provides the repository artwork.

## Contributing

Issues and pull requests are welcome. Keep proposed rules broadly useful, concise enough for repeated loading, and compatible with specialist skills.

## License

Released under the [MIT License](./LICENSE).
