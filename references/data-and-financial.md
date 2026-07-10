# Data And Financial Logic

Read this reference when working on data pipelines, crawlers, market data, scoring systems, portfolio logic, or financial calculations.

## Separate Data Stages

Keep these stages distinct when practical:

1. Raw external input
2. Validated and normalized data
3. Derived metrics
4. Scores or signals
5. Final decisions or persisted output

Do not let raw provider fields leak through the full application when a stable internal model would make behavior easier to audit.

## Make Calculations Auditable

- Write formulas explicitly.
- Name intermediate values by domain meaning.
- Replace hidden magic constants with named configuration or documented constants.
- Preserve source, timestamp, unit, currency, and transformation metadata when relevant.
- Keep rounding, conversion, and missing-value rules visible.
- Add focused tests for boundary values and representative examples.

Prefer names such as:

- `valuation_score`
- `growth_score`
- `risk_penalty`
- `composite_score`
- `portfolio_snapshot`

Avoid names such as `score1`, `factor_a`, or `final_final_score`.

## Handle Missing And Stale Data

- Never fabricate missing observations.
- Do not silently replace missing values with zero or another misleading default.
- Distinguish missing, stale, partial, and invalid data.
- Make partial pipeline failures visible in outputs and operational status.
- Define whether downstream calculations should stop, degrade, or continue with an explicit warning.

## Protect Sensitive Data

Never log account identifiers, transaction details, credentials, tokens, cookies, or private financial records unless the project has an explicit, reviewed policy for doing so.

Use synthetic or redacted fixtures in tests and examples.
