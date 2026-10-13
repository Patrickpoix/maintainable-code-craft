# Implementation and reviewability reference

Load this reference when implementation quality itself is the hard part: a large/cohesive parser or workflow, error/resource semantics, environment-dependent values, dependency choice, human reviewability, temporary workarounds, or a non-trivial semantic diff. Do not load it for every tiny code edit.

## Implement one coherent semantic batch

- Do not mix a behavior change with unrelated formatter sweeps, generated metadata churn, broad lint cleanup, or opportunistic refactors.
- Prefer direct, domain-named, local, predictable code over clever generic abstractions.
- Name data by its real semantics and unit. Do not call characters tokens, inferred time measured time, cached data live data, or an approximation an exact metric merely because the stronger name is convenient.
- Keep a larger cohesive parser, validator, or workflow when splitting would hide its contract; split when independent callers, side effects, authority, or change axes are genuinely mixed.
- Use guard clauses where they clarify invalid or terminal cases; do not mechanically flatten every branch.
- Batch file/database/external work when the real contract permits it; do not obscure correctness or audit semantics just to optimize a loop.
- Never swallow failure or return fake success. Preserve the primary cause with useful operation/resource context without leaking secrets.
- Give external calls and subprocesses bounded timeout/cancellation behavior when hanging violates the caller contract. Route a real retry/partial-data/user-facing error-contract design to `error-handling` rather than growing generic fallback machinery here.
- Keep policy separate from transport, storage, browser, and framework mechanisms.
- Follow the repository's language/runtime conventions. Framework/runtime/version behavior that changes independently of the repository should come from current official documentation rather than a remembered tutorial.

## Environment-dependent values and reproducibility

Do not hardcode a value whose correct value can change independently of source code. Machine paths, hosts/base URLs, ports, external IDs, provider/model names, tunable thresholds, retry/timeout limits, schedules, date/time windows, account/tenant/environment defaults should come from the repository's existing config/env/input/data contract/canonical owner.

Do not cargo-cult literal elimination. Protocol-mandated values, HTTP status codes, enum/discriminated-union tags, schema field names, mathematical constants, and isolated test/example fixtures may remain literal when that is the clearest representation. Stable non-obvious domain invariants belong as named constants at their natural owner, not in a generic constants Registry.

Make time, randomness, locale, units, rounding, missing-data policy, and concurrency explicit when they affect correctness or reproducibility.

Preserve meaningful epistemic states in types and projections. Unknown, unavailable, failed, omitted intentionally, not measured, stale, empty, and measured zero should remain distinguishable for as long as the owning contract distinguishes them; placeholders such as `0`, `[]`, or `""` must not fabricate certainty.

## Temporary workaround lifecycle

A diagnostic workaround can help isolate a root cause; it is not automatically shippable.

- **Root cause solved:** remove the workaround.
- **Real long-term host/platform constraint:** rewrite it into the clearest natural implementation and document the non-obvious reason nearby.
- **Still diagnostic-only:** do not present the responsibility as cleanly complete.

Do not permanently encode shell, encoding, quoting, or path mishaps into unreadable production code merely because the workaround executes.

## Evidence budget and test complexity

Verification should falsify a product claim, not grow a second product around the claim. Before adding durable audit/evidence machinery, answer three questions:

1. Which concrete failure mode can this layer falsify that the current real-caller observation or focused test cannot?
2. Why is a real runtime falsifier, canonical-boundary assertion, or existing test insufficient?
3. If this layer were deleted after the task, which independent guarantee would be lost?

If the third answer is only "more confidence", do not keep the layer. Prefer, in order, the smallest trustworthy mechanism that proves the actual boundary:

`real runtime falsifier -> canonical-boundary assertion -> focused test -> lightweight evidence receipt -> custom validator/proof subsystem`

The order is not absolute—static provenance or release identity can legitimately need dedicated validation—but moving right requires a real property that the simpler mechanism cannot establish.

- A fixture may synthesize inputs or external dependencies; it should not reproduce the product's state machine, parser, policy engine, ownership graph, or semantic decision rules and then use that duplicate model as the oracle.
- Keep orchestration, observation, and assertion visibly separate in long E2E tests. When setup such as launch/create/send/poll/restart/readback/dispose repeats, extract only the smallest stable lifecycle helper after the repetition is proven.
- Evidence receipts record observed facts and provenance. They should not infer a stronger product truth than the test actually observed.
- A validator that exists mainly to validate another validator is suspect. Retain it only when it protects an independent trust/provenance boundary or catches a demonstrated class of false PASS.
- Hand-written parsers or semantic analyzers need unusually strong justification when an existing parser/AST/runtime check can establish the property more directly and with fewer language-edge assumptions.

## Construction residue and reduction pass

Exploratory probes are allowed while a boundary is unknown. Their lifecycle is different from retained tooling:

- **Explore:** create the smallest disposable probe needed to distinguish competing hypotheses; prefer the OS temp directory when repository persistence is unnecessary.
- **Converge:** once one canonical reusable test/tool proves the responsibility, migrate any unique useful behavior into it.
- **Reduce:** delete superseded probes, debug scripts, temporary adapters, stale receipts, and near-duplicate experiments before closure unless each retained artifact has a distinct continuing caller.

The cost already spent writing a diagnostic artifact is never justification for keeping it.

For a non-trivial engineering packet, do one compact reduction pass before the final verification claim:

**Production**
- Is there a second owner, dead forwarding wrapper, duplicate rule, test-only production API, or speculative guard/fallback with no supported failure model?

**Tests and evidence**
- Did a fixture copy product semantics? Is lifecycle setup duplicated enough for one thin helper? Does any proof layer add no independent guarantee?

**Tooling**
- Is a probe superseded? Is a one-off script still in the repository? Does one validator merely restate another validator's result?

**Repository**
- Are stale evidence, obsolete comments/docs, generated/debug output, or temporary files left behind by the task?

For long tasks, summarize the result briefly in the handoff when useful:

`Reduction: removed / collapsed / retained because / remaining justified complexity`

This is a closure aid, not a mandatory new document or ceremony layer. Re-run only the evidence invalidated by reduction; do not repeat broad validation mechanically.

## Dependencies and human reviewability

Check standard-library and already-installed capabilities before adding a package. Add a dependency only when it materially reduces current complexity or risk and fits maintenance/licence constraints.

**Structure comes before commentary.** When a human reviewer would struggle, first reduce mixed responsibilities, bad names, avoidable nesting, repeated state/conditions, and over-wide data shapes. Use comments/docstrings only for legitimate complexity that remains.

Comment density follows cognitive complexity, not LOC. Simple code needs no ceremony. For a materially complex file, a short reading entry may identify responsibility/non-responsibility, main flow, important inputs/outputs/side effects, owner, and external boundary. Explain non-obvious why, invariants, ordering, failure/authority semantics, platform/provider quirks, and complex calculations or state transitions when structure and names cannot make them obvious.

Prefer concise Simplified Chinese comments for user-maintained local code when project-local policy does not require another language. Do not translate obvious syntax, duplicate a design document, or introduce comment quotas/KPIs. Stale commentary is a defect.

A durable comment answers what a future refactor would otherwise get wrong:

```ts
// Bad: restates the code.
// Update the session state.

// Good: owner, rejected alternative, prevented failure.
// The session owns the effective model for the whole run. Reading the global
// catalog here would let a mid-run refresh change execution semantics, so the
// run keeps the snapshot it was admitted with.
```

Keep source comments to durable rationale needed to modify the code safely. Investigation chronology, discarded hypotheses, and superseded diagnoses belong in commit/worklog/evidence history rather than remaining as permanent source commentary unless that history itself is required to understand an invariant.

For important calculations, make units, sign/direction, denominator/window/intermediate meaning, and special boundaries understandable from the owner. For cross-module stages, structure/names/types/comments should make `producer -> current owner -> consumer` traceable without a mechanical caller list.

## One semantic diff review

Before completion, reread the candidate once through four lenses:

1. **Behavior** — did output, persisted/public shape, ordering, authority, or error/failure semantics change beyond the intended claim?
2. **Ownership** — does the real caller now reach exactly one canonical owner, or did an old wrapper/fallback/constant/producer survive?
3. **Human reviewability** — did cognitive width, state plumbing, and unnatural control flow actually decrease? Can a first-time reviewer understand the responsibility and non-obvious invariants without excessive reverse-engineering?
4. **Diff scope** — is every changed line needed for the accepted claim, or did unrelated cleanup, formatting/generated churn, defensive ceremony, compatibility, comment noise, debug code, or correction residue remain?

This is one evidence-oriented reread, not four separate review rituals. A green test does not override a failed lens; correct the candidate and rerun only evidence invalidated by that correction.

