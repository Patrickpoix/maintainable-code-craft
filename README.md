# Maintainable Code Craft

A lightweight engineering skill for better software design and implementation. It encourages whole-system understanding, clear ownership, natural code, and thoughtful verification **without prescribing a coding workflow**.

## Philosophy

Good engineering is not the smallest diff, the fewest lines, or the longest checklist. It is a solution whose scope and structure match the real problem.

- **Understand the system.** Trace relevant callers, data, state, dependencies, and user-visible behavior. For broad goals, consider the desired end state before accumulating local fixes.
- **Choose the right responsibility.** Keep facts and rules with their natural owner, remove dead mechanisms, and simplify unnecessary abstractions. Refactor across modules when that is the coherent solution.
- **Write naturally.** Prefer idiomatic, direct code, meaningful interfaces, and comments that explain non-obvious decisions.
- **Preserve what matters.** Be deliberate about behavior, errors, units, persistence, and authorization. Verify consequential changes using meaningful evidence; avoid ceremonial checks.
- **Judge the outcome.** Optimize for correctness, maintainability, performance, and a consistent whole—not metrics such as LOC or test counts.

This is a **judgment aid**, not a rigid method. Small tasks may need only a simple fix; large architectural problems can warrant broad changes. Project-specific contracts, safety rules, and user authorization always take precedence.

## Installation and use

Install the repository in your coding assistant's Skill directory with `SKILL.md` at its root. The Skill may be used implicitly or invoked explicitly:

```text
Use maintainable-code-craft to review the system and propose the clearest complete solution.
```

```text
Refactor this subsystem for simpler ownership and maintainability without losing its behavior.
```

The Skill is framework-agnostic and contains no mandatory process artifacts, metrics, or specialist-Skill dependencies.

## License

MIT. See [LICENSE](LICENSE).
