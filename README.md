# Maintainable Code Craft

[English](./README.md) | [简体中文](./README.zh-CN.md)

![Maintainable Code Craft](./assets/banner.svg)

[![License: MIT](https://img.shields.io/badge/License-MIT-1f2937.svg)](./LICENSE)
[![Codex Skill](https://img.shields.io/badge/Codex-Skill-0f766e.svg)](./agents/openai.yaml)
[![Focus: Maintainability](https://img.shields.io/badge/Focus-Maintainability-b45309.svg)](./SKILL.md)

Write code that still feels good to read three months later.

`maintainable-code-craft` is a reusable Codex skill for teams who want AI-assisted code to feel calm, readable, and reviewable instead of clever, noisy, or over-engineered.

It pushes Codex toward:

- Better names
- Smaller changes
- Clearer structure
- Safer configuration
- More explicit errors
- Code a human teammate can confidently maintain

## Default Baseline Skill

For code-writing and code-changing work, this skill is meant to act as a default baseline skill.

That means:

- It is the stable foundation for maintainability, naming, structure, scope control, and engineering taste
- It is usually used together with specialist skills instead of replacing them
- It is non-exclusive by design
- It should remain active when Codex is writing, editing, refactoring, debugging, or reviewing code

In practice, `maintainable-code-craft` often works alongside skills such as:

- `tdd-workflow` for test-driven feature work and bug fixes
- `error-handling` for retries, fallback logic, and failure design
- `verification-loop` for final validation before shipping
- Database-specific skills for schema, query, and migration work
- UI or design skills for frontend-heavy tasks

Think of it as the maintainability layer, not the task-specific specialist.

## Why People Use It

AI can make code fast. That does not always mean it makes code pleasant to own.

This skill exists for the moment when you want Codex to stop optimizing for novelty and start optimizing for engineering taste:

- Prefer the simplest design that safely solves the real task
- Preserve local codebase conventions instead of rewriting everything
- Make side effects obvious
- Keep formulas, scoring, and data logic auditable
- Avoid vague names, hidden magic, and dependency sprawl

If you want "boring in the best possible way," this skill is for you.

## What It Changes In Practice

Instead of output like this:

```text
Refactor this into a flexible manager with helper abstractions and utility layers.
```

You steer Codex toward output like this:

```text
Keep the change small. Use clear names. Make file writes explicit. Do not introduce new abstractions unless they remove real complexity.
```

The result is usually:

- Code that is easier to review
- Diffs that are easier to trust
- Fewer accidental rewrites
- Less hidden behavior
- Better handoff to the next developer

## Best Fit

Use this skill for:

- Application code
- Scripts and automation
- Tests
- Crawlers and API clients
- Data pipelines
- Financial and scoring logic
- UI and dashboard code
- Refactoring and review work

## Core Principles

- Name things by intent, not by placeholder
- Keep functions focused on one responsibility
- Make errors visible and actionable
- Keep raw data, normalized data, and derived results separate
- Prefer local consistency over generic "best practice" churn
- Add dependencies only when they clearly reduce risk or complexity

## Quick Start

Example prompts:

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

## Install

### Global install

Place this directory at:

```text
~/.codex/skills/maintainable-code-craft/
```

### Project-local install

Place this directory at:

```text
<your-project>/.agents/skills/maintainable-code-craft/
```

## Repository Layout

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

## Included Files

- `SKILL.md` contains the full maintainability workflow and writing rules.
- `agents/openai.yaml` contains Codex-facing metadata for the skill interface.
- `assets/banner.svg` provides the repository banner used in this README.

## Notes

This repository contains only the files needed to reuse the skill. It does not include private Codex configuration, unrelated local skills, credentials, or personal environment data.

## License

MIT. See [LICENSE](./LICENSE).
