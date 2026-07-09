# Maintainable Code Craft

A reusable Codex skill for writing code that is clear, maintainable, and easy to debug.

This skill is designed as a default engineering baseline for code writing, editing, reviewing, refactoring, and debugging work. It emphasizes small safe changes, explicit naming, clear error handling, auditable logic, and avoiding unnecessary complexity.

## What This Skill Does

`maintainable-code-craft` helps Codex produce code that is:

- Human-readable
- Consistent with the local codebase
- Conservative in scope
- Explicit about side effects and errors
- Careful with configuration and secrets
- Easier to maintain over time

It is a good fit for:

- Application code
- Scripts and automation
- Tests
- Crawlers and API clients
- Data pipelines
- Financial and scoring logic
- UI and dashboard code
- Refactoring and review work

## Repository Layout

```text
maintainable-code-craft/
  SKILL.md
  agents/
    openai.yaml
  README.md
  LICENSE
```

## Install

### Option 1: Global install

Copy this folder to:

```text
~/.codex/skills/maintainable-code-craft/
```

### Option 2: Project-local install

Copy this folder to:

```text
<your-project>/.agents/skills/maintainable-code-craft/
```

## Usage

Use this skill when you want Codex to favor maintainable engineering decisions during coding work.

Typical prompts:

```text
Use maintainable-code-craft to refactor this module without changing behavior.
```

```text
Use maintainable-code-craft to review this patch for naming, structure, and maintainability issues.
```

```text
Use maintainable-code-craft while implementing this feature and keep the change small and readable.
```

## Included Files

- `SKILL.md`: the main maintainability rules and workflow
- `agents/openai.yaml`: Codex interface metadata

## Notes

This repository contains only the skill files needed for reuse. It does not include your private Codex configuration, other installed skills, or personal environment data.

## License

MIT
