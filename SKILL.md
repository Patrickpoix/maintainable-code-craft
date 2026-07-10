---
name: maintainable-code-craft
description: Apply a default, non-exclusive maintainability baseline whenever Codex writes, edits, refactors, debugs, reviews, or generates code or configuration. Use alongside task-specific skills for application code, scripts, tests, automation, APIs, data pipelines, UI code, and architecture work. Emphasize clear naming, small focused changes, idiomatic local style, explicit errors, safe configuration, justified dependencies, and avoiding over-engineering.
---

# Maintainable Code Craft

Write code for the teammate who will debug it months from now.

Prefer clear, idiomatic, unsurprising code over cleverness or demo appeal. Choose the simplest design that safely solves the real task.

## Baseline Role

Use this skill as the maintainability layer, not as a replacement for specialist skills.

- Keep it active for code writing, editing, refactoring, debugging, and review.
- Combine it with task-specific skills when they add a deeper workflow.
- Let specialist guidance control its domain while this skill controls naming, structure, scope, and engineering taste.

Common combinations include:

- `tdd-workflow` for feature work and bug fixes
- `error-handling` for retries, fallbacks, and failure design
- `verification-loop` for final validation
- Database-specific skills for schemas, queries, migrations, and ORMs
- UI and design skills for interface-heavy work
- Production and security skills for operational risk

## Working Sequence

Before editing an existing project:

1. Inspect the relevant structure, naming, imports, configuration, errors, and tests.
2. Identify the smallest change that satisfies the request.
3. Preserve local conventions unless they are clearly harmful.
4. Implement without unrelated rewrites, formatting churn, or dependency changes.
5. Verify the changed behavior at a level proportional to its risk.

Introduce an abstraction only when it removes real duplication, clarifies a domain boundary, or isolates unstable external behavior.

## Naming

Choose names that explain intent and domain meaning.

Prefer names such as:

- `account_summary`
- `calculate_total_price`
- `is_cache_stale`
- `save_report_snapshot`

Avoid vague names such as:

- `data`
- `thing`
- `stuff`
- `obj`
- `result2`
- `final_final`
- unexplained abbreviations
- pinyin identifiers

Use short names such as `i`, `row`, or `tmp` only in tiny scopes where their meaning is obvious.

Name booleans as statements, such as `is_active`, `has_valid_token`, `can_retry`, or `should_refresh`.

## Functions And Side Effects

Keep each function focused on one responsibility. Separate validation, fetching, normalization, calculation, persistence, and presentation when combining them would hide behavior.

Make side effects obvious in names:

- `write_manifest_file`
- `update_crawler_state`
- `delete_stale_cache`
- `send_report_email`

Do not hide file writes, database updates, network calls, browser automation, or global state changes behind names that sound pure.

## Structure

Use structure only when it improves navigation or separates responsibilities.

For medium or large projects, separate configuration, external clients, data access, business logic, entrypoints, and tests when those concerns evolve independently.

Do not force a multi-folder architecture onto a small script. Grow structure when the current shape starts hiding responsibilities.

## Errors And Configuration

Never silently swallow errors or return fake success after a failure.

Handle expected failures deliberately, including invalid input, missing configuration, malformed data, timeouts, rate limits, authentication failures, permission issues, and empty external responses.

Write error messages that explain what failed and what the next developer should check.

Never hard-code or log secrets, tokens, cookies, passwords, account credentials, user-specific absolute paths, or environment-specific ports. Use the project's existing configuration system; if none exists, prefer environment variables or a small documented config file.

## Types And Contracts

Use explicit contracts at system and module boundaries.

- Type public functions and important internal functions when the language supports it.
- Keep external response shapes separate from normalized domain models when practical.
- Avoid unstructured dictionaries or `any` values crossing multiple layers without validation.
- Make optional, missing, stale, and partial states explicit.

## Domain Guidance

For data pipelines, scoring systems, market data, portfolio logic, or financial calculations, read [references/data-and-financial.md](references/data-and-financial.md) before editing the relevant logic.

For substantial UI, database, security, testing, or deployment work, use the appropriate specialist skill instead of expanding this baseline with domain-specific instructions.

## Tests And Dependencies

Add or update focused tests for bug fixes, parsers, calculations, validation, state transitions, and edge cases when feasible.

Do not rely on live external services for normal unit tests unless the project already provides integration-test infrastructure.

Before adding a dependency:

1. Check whether the standard library is enough.
2. Check whether an existing dependency is enough.
3. Add a dependency only when it clearly reduces risk or complexity.
4. Explain why it is justified.

## Comments

Write comments to explain why, not what. Use them for business rules, external quirks, compatibility constraints, non-obvious tradeoffs, and intentional limitations.

Do not bury straightforward code under commentary.

## Final Check

Before finalizing, confirm that:

- Names are clear and human-readable.
- Functions are focused and side effects are visible.
- Existing project style is preserved.
- The change stays within the requested scope.
- Errors are explicit and actionable.
- Secrets and environment-specific values are not hard-coded or logged.
- Dependencies are justified.
- Important behavior has tests or clear manual verification.
- Missing, stale, and partial data are handled deliberately.
- The solution is no more complex than the problem requires.

For meaningful changes, summarize what changed, why it fits the codebase, how it was verified, and any remaining risk. Keep tiny edits concise.
