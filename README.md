# Maintainable Code Craft

![Maintainable Code Craft](./assets/banner.svg)

[![License: MIT](https://img.shields.io/badge/License-MIT-1f2937.svg)](./LICENSE)
[![Codex Skill](https://img.shields.io/badge/Codex-Skill-0f766e.svg)](./agents/openai.yaml)
[![Focus: Maintainability](https://img.shields.io/badge/Focus-Maintainability-b45309.svg)](./SKILL.md)

A reusable Codex skill for writing code that is clear, maintainable, and easy to debug.

This repository packages `maintainable-code-craft` as a portable skill that can be installed globally or per project. It helps Codex favor readable structure, explicit naming, safe configuration, focused changes, and maintainable engineering choices across day-to-day coding work.

## Why This Skill Exists

Many AI-assisted coding workflows can drift toward code that is flashy, overly abstract, or difficult to maintain. This skill pushes in the opposite direction.

It helps Codex write code that is:

- Clear and human-readable
- Consistent with the local codebase
- Small in scope and safer to review
- Explicit about side effects and errors
- Careful with configuration and secrets
- Easier to maintain months later

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

## What It Encourages

- Clear domain naming instead of vague placeholders
- Small, focused functions and changes
- Local consistency over unnecessary rewrites
- Explicit errors over silent failure
- Safe configuration instead of hard-coded secrets
- Auditable formulas and data logic
- Justified dependencies instead of trend-driven additions

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

## Repository Layout

```text
maintainable-code-craft/
  assets/
    banner.svg
  SKILL.md
  agents/
    openai.yaml
  README.md
  LICENSE
```

## Included Files

- `SKILL.md` contains the maintainability rules and workflow guidance.
- `agents/openai.yaml` contains Codex-facing metadata for the skill interface.
- `assets/banner.svg` provides the repository banner used in the README.

## Notes

This repository contains only the files needed to reuse the skill. It does not include private Codex configuration, unrelated local skills, credentials, or personal environment data.

## License

MIT. See [LICENSE](./LICENSE).
