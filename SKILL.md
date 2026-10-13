---
name: maintainable-code-craft
description: "Default engineering method for implementing, modifying, refactoring, or code-reviewing software and engineering config / 代码实现、修改、重构、代码审查. Not for unknown-root-cause debugging, test-first or regression design, security review, completion verification, or prose-only plans."
---

# Maintainable Code Craft

Use this as the default engineering discipline for code work. The aim is not to apply more patterns; it is to make the sequence of decisions resemble a careful maintainer: understand the current system, decide whether code should exist, freeze enough of the design to avoid inventing architecture mid-edit, make the smallest **sufficient** durable vertical change that fully closes the selected responsibility, inspect the real result, remove temporary complexity, verify the changed claim, clean the workspace, and stop at the correct boundary.

Project `AGENTS.md`, project contracts, user instructions, and the real caller remain authoritative. Global/project AGENTS own hard policy such as architecture existence, capability honesty, compatibility, safety, and authorization; this skill focuses on executing those policies through a natural engineering lifecycle. `verification-loop` owns generic verification evidence, while specialist/project gates own their narrower verdicts.

The phases below are a **mental work cycle, not a requirement to create plans, matrices, registries, or worklogs**. Collapse them naturally for small tasks and expand them only when risk or scope warrants it.

Treat optional uncertainty reduction as optional. Extra checks, review passes, hardening, or evidence artifacts enter the main path only when they directly serve the requested outcome, can change a current decision, settle a still-live uncertainty, or protect a current caller/contract/boundary. "This could improve confidence" is not by itself a reason to add work; normal implementation abstractions are judged by the real-caller and complexity rules below.

## Default reflexes

When the pull is toward the left column, take the right one unless current evidence says otherwise:

| Tempting move | Default |
| --- | --- |
| Add a helper, guard, validator, or adapter to contain a risk | Find the owner whose contract makes the risk impossible and fix it there |
| Keep the old path as a fallback "until later" | Cut real callers over and delete it in the same change; keep only a real external/persisted edge, with an owner and removal condition |
| Split a function because it is long | Split where callers, side effects, or authority differ: a cohesive 80-line parser with one caller stays whole; a 15-line function that decides policy, writes storage, and emits events splits |
| Add state so verification or recovery can check something | Read the owner's existing fact; projections render, they do not store |
| Land one responsibility as several checkpoint patches | Land one complete ownership unit; checkpoint only at a real recovery boundary |
| Guard against a plausible but unobserved failure | Name the failure and what produces it; if you cannot, do not add the guard |
| Comment what the code does | Comment why this owner, which simpler-looking alternative is wrong, and what real failure the invariant prevents |
| Add tests or checks for confidence | Add the one check that fails if the claim is false |

Investigation may be as adversarial and exhaustive as the problem needs (`systematic-debugging`); the shipped diff contains only what the chosen owner needs. Evidence gathered to find the owner does not become production code.

## JIT references

Keep the entry lightweight. Load a reference only when the current responsibility needs its detail:

- `references/architecture-and-refactoring.md` — non-trivial architecture, ownership migration, defensive machinery, compatibility, large decomposition, or repository refactor.
- `references/implementation-and-review.md` — difficult implementation shape, environment-dependent values, dependency choice, comments/reviewability, workaround cleanup, or a non-trivial semantic diff.
- `references/long-work-and-boundaries.md` — multi-session work, runtime-owned checkout, recovery boundary, persisted-data lifecycle, task-owned process cleanup, or an external future gate.

Do not preload these references merely because they exist. The main entry is sufficient for ordinary implementation and local refactoring.

## 1. ORIENT & UNDERSTAND — establish action-time reality

Before a non-trivial edit, establish only the current facts needed to act: workspace/root, branch/worktree and dirty state when relevant, project instructions, current runtime/executable when it owns behavior, and the user's requested outcome. Current source/runtime/persisted truth outranks stale chat, plans, Worklogs, or remembered architecture.

Read in the direction of actual use:

`user outcome -> real caller -> current input -> canonical owner -> data/control flow -> observable output`

- Inspect the relevant entrypoint, owner, adjacent implementation, configuration, error path, callers, and local conventions before inventing an abstraction.
- Treat entrypoints, routes, handlers, CLI commands, UI actions, and compatibility shims by their observable contract, not by their filename or implementation shape. A thin wrapper may still be the shipped external boundary; an apparently substantial module may be dead if no real caller reaches it. Remove or replace one only after tracing the actual caller and preserving or deliberately retiring the behavior it owns.
- Identify the semantic owner of facts that cross layers—state, metrics, identity, provenance, freshness, or authority—not only the function that computes them. A projection may render or transport an owned fact; it should not quietly become a second source of truth.
- Preserve epistemic distinctions the caller can observe. `missing`, `empty`, `unavailable`, `failed`, `stale`, `unknown`, `not measured`, and measured zero are different claims; do not collapse them for convenience.
- Tests, docs, barrels/re-exports, audits, and historical artifacts are evidence, not substitutes for a live production caller.
- Resume existing work from current persisted/worktree state rather than reconstructing a second implementation from memory.
- Distinguish source, config, tests, generated output, caches, runtime state, experiments, backups, and history; co-location does not give them one lifecycle.
- When a failure may be environmental, classify source logic vs configuration/runtime/shell/encoding/path/cwd/invocation/external dependency before rewriting product code. Preserve the project's primary language and existing stack unless evidence requires change.
- Prefer a known working local behavior or accepted product artifact over recreating a generic best-practice version.
- Compare external alternatives only when the task genuinely introduces a new architecture/dependency/infrastructure capability.

If caller liveness, defensive machinery, compatibility, or architecture survival is the actual question, load `references/architecture-and-refactoring.md`.

## 2. DECIDE & DESIGN — choose the shortest durable path

Prefer subtraction and native capability first: can the current canonical owner solve the problem directly, can obsolete code be retired, or can a native/runtime capability replace a custom Manager/Registry/Adapter/cache/recovery layer? Complexity needs a present owner and consequence, not hypothetical flexibility.

Freeze enough design before substantive edits to answer:

`requested outcome -> real caller -> current owner -> target owner -> key input/output/side effects -> protected invariants -> ordered cutover -> smallest falsifying proof`

This is a working model, not a required plan artifact. Small fixes collapse naturally; non-trivial work should not use production edits as the mechanism for discovering architecture.

- Keep one authoritative implementation per business rule or transformation. DRY semantics, not superficial syntax.
- Before adding a cache, memoizer, index, alternate fast path, or optimization framework, measure the real caller cost with equivalent semantics. An immaterial cost is evidence to keep the simpler design.
- Keep orchestration readable as workflow stages; stage code owns calculations/side effects, orchestration owns sequencing and stop conditions.
- Pass explicit named facts instead of hidden globals or giant generic context bags. An extraction should retire real cognitive burden, not just move the same state across another file boundary.
- Define meaningful empty/partial/blocked/failed/complete states when callers distinguish them.
- Validate at the canonical trust boundary, then trust the established internal contract until a new independent boundary appears.
- Compatibility belongs at the real external/persisted edge and needs an owner plus removal condition; do not preserve dead internal shape merely because tests import it.

For substantial structural refactors, preserve this order:

`module internal -> module connection -> physical move`

Stabilize responsibility at the current path, make producer/consumer handoff explicit and cut real callers to one owner, then move files. Do not mass-move a tree and use compiler/import failures to discover architecture. Treat unexpected blast radius as evidence of a wrong seam or hidden dependency, not permission for cascade patching.

Load `references/architecture-and-refactoring.md` when these condensed rules are not enough for the current architecture/refactor decision.

## 3. IMPLEMENT & REFINE — make one natural semantic change

Make one coherent semantic batch at a time. Do not mix a behavior change with unrelated formatter sweeps, generated metadata churn, broad lint cleanup, or opportunistic refactors.

- Prefer direct, domain-named, local, predictable code over clever generic abstractions.
- Treat names as contracts: fields, APIs, metrics, and UI labels describe what is actually known or measured, not a convenient approximation that future code could mistake for stronger truth.
- Use guard clauses where they clarify invalid or terminal cases; do not mechanically flatten every two-way branch.
- Never swallow failures or return fake success. Preserve the primary cause and enough operation/resource context to diagnose it without leaking secrets.
- Give external calls/subprocesses bounded timeout/cancellation behavior when hanging violates the caller contract. Add `error-handling` only when a real retry/partial-data/user-facing failure contract needs design.
- Keep policy separate from transport/storage/browser/framework mechanism and follow repository-local language/runtime conventions before importing generic style rules.
- Values that change independently of source belong to the existing config/env/input/data-contract owner; stable protocol/schema/enum/math constants and isolated fixtures may remain literal when that is clearer.
- Make time, randomness, locale, units, rounding, missing-data policy, and concurrency explicit where they affect correctness or reproducibility.

After the first working implementation, reread the changed responsibility as a maintainer before declaring it done.

Look specifically for:

- duplicated business rules or divergent status vocabularies;
- one-line forwarding wrappers and abstractions that reduced no concept;
- oversized context bags or hidden mutable state;
- excessive validation after a trusted boundary;
- temporary debug code, imports, compatibility, feature flags, or workarounds;
- awkward names, surprising side effects, or implementation order that obscures the real workflow;
- accidental EOL/formatter/generated churn mixed into the semantic diff.
- over-correction: a local user/reviewer objection should normally produce a local correction; reverse the broader direction only when evidence invalidates the premise that direction depended on.

A refactor succeeds when concepts, rule implementations, branch burden, or state ambiguity decrease—not merely when a function becomes shorter.

Before completion, perform one semantic diff review across **behavior, ownership, human reviewability, and diff scope**. A green test cannot excuse a failed lens. Load `references/implementation-and-review.md` when comments, dependency choice, workaround lifecycle, environment-dependent values, or the detailed diff review is material.

## 4. REDUCE — spend complexity only where it buys an independent guarantee

After the changed responsibility works, reduce the candidate before closure. The default engineering cycle is:

`understand owner -> smallest sufficient change -> implement -> falsify -> reduce -> reverify invalidated evidence -> close`

Do not let a normal repair silently grow into `helper -> validator -> manifest -> recorder -> validator-of-validator`. Every retained layer must own a current responsibility or an independent guarantee that would actually disappear if that layer were deleted.

- Verification code may drive and observe product contracts; it must not become a second implementation of the product state machine, parser, policy engine, ownership model, or semantic rules merely to prove them.
- Reduction is not LOC golf. Preserve every demonstrated correctness, safety, compatibility, provenance, or recovery property that still has a real owner.

For non-trivial test/evidence or workaround-heavy changes, load `references/implementation-and-review.md` for the evidence budget, construction-residue lifecycle, and reduction checklist.

## 5. CLOSE — observe, verify, clean, and stop at the right boundary

“Code executed” is not the same as “the requested result works.” Observe the surface the caller actually consumes when feasible, then hand the evidence claim to `verification-loop`, which owns boundary selection, depth, freshness, and the completion verdict. Do not activate `tdd-workflow` automatically; route regression design to `ai-regression-testing` only when a real repeated/shared contract justifies it. Never claim an unrun test/audit/build/deployment passed.

Clean task-owned construction residue while preserving unrelated and unknown assets. Remove temporary debug/generated/workaround material when ownership is proven and stop task-owned processes only through verified identity and their graceful lifecycle.

If a long or multi-session responsibility needs a recovery boundary, or the remaining gate belongs to wall-clock time, an external service/provider, authentication/permission, data arrival, or a human, load `references/long-work-and-boundaries.md`. Record the concrete blocker/re-entry fact and stop instead of polling or inventing adjacent work.

Persisted-data, destructive-action, security, financial, publication, research/PIT, and protected-asset hard rules remain owned by global/project policy and their specialist Skills. Do not duplicate or weaken them here.

Scale naturally. A small fix may be only:

`understand caller -> fix -> inspect -> verify -> clean -> stop`

Expand the cycle only with demonstrated risk or scope; process volume is not rigor.

A supported no-change conclusion is a valid engineering result when current evidence shows the suspected cost, defect, or abstraction need is immaterial or absent. Do not manufacture work to make the review look productive.

## Specialist routing

Use the smallest compatible set of specialist skills:

- unknown/cross-component root cause -> `systematic-debugging`;
- real retry/timeout/partial-data/error-contract design -> `error-handling`;
- repeated regression category or shared-contract regression protection -> `ai-regression-testing`;
- database/schema/data migration -> `database-migrations` plus the relevant DB/ORM skill;
- browser or Windows GUI flow -> the appropriate E2E skill.

Do not stack workflow skills merely because several descriptions sound relevant. The quality of engineering judgment comes from choosing the right boundary, not loading the largest checklist.

## Completion standard

Before claiming the task complete, confirm that:

- the requested outcome is actually closed, not merely the user's enumerated steps; no known in-scope caller defect, half-wired path, duplicate owner, or obvious temporary workaround is left solely because the user did not explicitly name that substep;
- the real caller reaches one authoritative implementation, and every layer the change added survived the reduction pass;
- task-owned residue is gone while unrelated assets and working lifelines were preserved;
- verification was actually run or explicitly left unrun with a reason;
- any remaining external/time/Human gate has a clear re-entry condition instead of an active polling loop.
