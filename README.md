# Maintainable Code Craft

A domain-neutral engineering skill for making software changes that remain understandable, testable, and easy to maintain.

It favors clear ownership, direct designs, explicit contracts, proportionate verification, and the smallest implementation that fully solves the current problem.

## What it is for

Use this skill when writing, editing, refactoring, reviewing, or generating code. It provides a general maintainability baseline while leaving architecture, business rules, safety policy, and operational procedures to the project that owns them.

The core workflow is deliberately simple:

`understand -> decide -> design -> implement -> observe -> refine -> verify -> clean`

For tiny changes, most of those steps collapse naturally. For larger changes, the same sequence expands only where risk or scope requires it.

## Principles

- Follow the real caller and current executable behavior.
- Keep one clear owner for each rule or transformation.
- Add abstractions only when they remove a real maintenance burden.
- Prefer direct code over speculative layers and generic wrappers.
- Separate responsibilities when they genuinely change independently.
- Validate at meaningful trust boundaries instead of everywhere.
- Make side effects and failure states explicit.
- Comment non-obvious reasoning, not obvious syntax.
- Observe the result at the boundary the caller actually consumes.
- Verify the changed claim with focused evidence.
- Preserve unrelated changes and remove task-owned residue.
- Keep project-specific rules in the project rather than embedding them in a global skill.

## Repository layout

```text
maintainable-code-craft/
├── SKILL.md
├── agents/
│   └── openai.yaml
├── README.md
├── README.zh-CN.md
├── LICENSE
└── assets/
    └── banner.svg
```

## Installation

Copy or clone this repository into the skill directory used by your coding assistant, keeping `SKILL.md` at the root of the skill folder. The exact skill location depends on the host application.

After installation, confirm that the host can discover a skill named `maintainable-code-craft`.

## Usage

The skill is designed to be usable as an implicit baseline or an explicit instruction. Typical requests include:

```text
Refactor this module using maintainable-code-craft.
```

```text
Review this implementation for unnecessary complexity and unclear ownership.
```

```text
Implement this change with the smallest complete design and focused verification.
```

Project-local instructions remain authoritative when they are more specific.

## Scope

This public skill intentionally stays general. It does not prescribe a particular framework, operating system, deployment method, business domain, data source, or release process.

That separation is deliberate: reusable engineering principles belong here; project-specific knowledge belongs with the project that owns it.

## License

MIT. See [LICENSE](LICENSE).