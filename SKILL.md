---
name: maintainable-code-craft
description: Apply maintainable, human-readable engineering standards whenever Codex writes, edits, refactors, debugs, reviews, or generates code. Use for application code, scripts, tests, crawlers, API clients, data pipelines, financial calculations, UI/dashboard code, automation, configuration, and architecture work. Emphasize clear naming, small focused changes, idiomatic local style, explicit errors, safe configuration, justified dependencies, auditable data logic, and avoiding over-engineering.
---

# Maintainable Code Craft

Write code like a careful engineer who expects to maintain this project for months.

Prefer code that is clear, idiomatic, boring in a good way, easy to debug, and consistent with the existing codebase.

Do not optimize for cleverness or demo appeal. Choose the simplest design that safely solves the real task.

## Working Rules

Before editing an existing project:

1. Inspect structure, naming, imports, configuration, error handling, and tests.
2. Preserve local conventions unless they are clearly harmful.
3. Make the smallest safe change that solves the task.
4. Avoid unrelated rewrites, formatting churn, and dependency changes.
5. Introduce abstractions only when they remove real complexity or isolate unstable external behavior.

When deeper guidance is needed and the relevant specialist skill is available, use it instead of duplicating the full workflow here:

* Use `error-handling` for complex error taxonomy, retries, fallback behavior, and user-facing failure design.
* Use `tdd-workflow` for feature work or bug fixes where tests should drive implementation.
* Use `verification-loop` before finalizing significant code changes.
* Use database-specific skills for schema, query, migration, and ORM work.
* Use UI/design skills for substantial frontend interface work.
* Use production or audit skills for deployment, security, reliability, and operational risk review.

This skill remains the default baseline for code taste, maintainability, naming, structure, and scope control.

## Naming

Use names that explain intent and domain meaning.

Prefer names like:

* `portfolio_snapshot`
* `valuation_score`
* `risk_adjusted_score`
* `fetch_seeking_alpha_ratings`
* `is_market_data_stale`
* `save_portfolio_nav_snapshot`

Avoid vague names like:

* `data`
* `thing`
* `stuff`
* `obj`
* `item`
* `result2`
* `final_final`
* unexplained abbreviations
* pinyin identifiers
* misleading names

Short names such as `i`, `row`, or `tmp` are acceptable only in tiny local scopes where the meaning is obvious.

Boolean names should read like true-or-false statements, such as:

* `is_active`
* `has_valid_token`
* `can_retry`
* `should_rebalance`
* `requires_refresh`

## Functions

Keep functions focused on one clear responsibility:

* validate input
* fetch data
* normalize data
* calculate a value
* transform a structure
* persist a result
* render output
* coordinate a small workflow

Avoid functions that fetch, transform, calculate, save, log, and render all at once.

Use side-effect names for side effects, such as:

* `save_to_database`
* `write_manifest_file`
* `update_crawler_state`
* `delete_stale_cache`
* `send_report_email`

Use calculation names for pure or mostly pure logic, such as:

* `calculate_weighted_score`
* `normalize_rating_history`
* `build_portfolio_snapshot`
* `rank_candidates_by_signal`

Do not hide file writes, database updates, network calls, browser automation, or global state changes behind names that sound pure.

## Structure

Use structure only when it improves clarity.

For medium or large projects, separate:

* configuration
* external clients
* data access
* business logic
* scoring or calculation logic
* UI/API/CLI entrypoints
* tests
* operational scripts

Do not force a multi-folder architecture onto a small one-file script.

Grow structure gradually when the current shape starts hiding responsibilities.

## Errors And Configuration

Never silently swallow errors.

Avoid:

* empty catches
* `except: pass`
* broad catch-all handlers without context
* fake success responses after failures

Handle expected failure modes deliberately:

* missing config
* invalid input
* missing files
* malformed data
* timeouts
* rate limits
* auth failures
* database failures
* empty external responses
* permission issues
* stale data

Use clear error messages with enough context for the next developer to know what failed and what to check.

Never hard-code:

* secrets
* tokens
* cookies
* passwords
* user-specific absolute paths
* environment-specific ports
* account credentials
* security codes

Use the project's existing config system. If none exists, prefer environment variables or a small config file.

Never log secrets, tokens, cookies, passwords, private account data, or sensitive financial account details.

## Types And Contracts

Use type annotations when the language supports them.

For Python:

* type public functions and important internal functions
* prefer typed domain objects over passing unstructured dictionaries through many layers
* keep raw external data separate from normalized internal models when practical

For TypeScript:

* define types for API responses, component props, and domain models
* avoid `any` unless there is a clear reason
* keep external response types separate from normalized internal types

For Go, Rust, Java, and similar languages:

* use explicit structs, classes, and domain types
* keep serialization formats separate from business models when helpful

## Data And Financial Logic

For data-heavy, crawler, market, portfolio, or financial code:

* keep raw data, normalized data, derived metrics, scores, signals, and final decisions separate
* make formulas explicit and auditable
* avoid hidden magic constants
* name intermediate values clearly
* preserve enough metadata to trace how an output was produced
* handle missing data deliberately
* do not fabricate data
* do not silently fill missing values with fake or misleading defaults
* make stale data and partial pipeline failures visible

Use names such as:

* `valuation_score`
* `growth_score`
* `profitability_score`
* `momentum_score`
* `revision_score`
* `risk_penalty`
* `composite_score`

Avoid names such as:

* `score1`
* `score2`
* `factor_a`
* `final_final_score`

## UI And Dashboard Code

For UI, dashboards, and app code:

* keep components focused
* separate data fetching from presentation when practical
* make loading, empty, error, stale, blocked, partial, and success states explicit
* avoid deeply nested conditional rendering
* use descriptive prop names
* keep operational state visible for crawlers, imports, scoring jobs, portfolio builds, and report generation

## Tests And Dependencies

When changing important logic, add or update focused tests when feasible, especially for:

* bug fixes
* parsers
* scoring formulas
* financial calculations
* data normalization
* validation logic
* state transitions
* API response mapping
* portfolio construction
* edge cases

Do not rely on live external services for normal unit tests unless the project already has integration-test infrastructure.

Before adding a dependency:

1. Check whether the standard library is enough.
2. Check whether an existing dependency is enough.
3. Add a new dependency only when it clearly reduces risk or complexity.
4. Explain why the dependency is justified.

Do not add dependencies only to make code look modern.

## Comments

Write comments to explain why, not what.

Useful comments explain:

* business rules
* external API quirks
* non-obvious tradeoffs
* compatibility constraints
* migration assumptions
* intentional limitations

Do not bury obvious code under excessive comments.

## Final Check

Before finalizing code, check:

* names are clear and human-readable
* no pinyin identifiers were introduced
* functions are focused
* side effects are obvious
* existing project style is preserved
* secrets and sensitive data are not hard-coded or logged
* configuration is not scattered
* errors are not silently swallowed
* dependencies are justified
* important logic has tests or clear manual verification
* data and financial formulas are auditable
* missing or stale data is handled deliberately
* the solution is not over-engineered

For meaningful code changes, summarize:

1. what changed
2. why this approach fits the codebase
3. how it was verified
4. any remaining risk

Keep tiny edits concise.
