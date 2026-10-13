# Architecture and refactoring reference

Load this reference only when the current responsibility involves a non-trivial architecture decision, decomposition, ownership migration, compatibility boundary, defensive machinery, or substantial repository refactor. Ordinary local implementation should stay on the main `SKILL.md` path.

## Trace the live system before changing structure

Read in the direction of actual use:

`user outcome -> real caller -> current input -> current owner -> data/control flow -> observable output`

- Treat tests, docs, re-exports, audits, and governance checks as evidence or support, not production callers that can justify an architecture by themselves.
- Distinguish caller liveness. A current executable, regenerator, runtime, or operator path is stronger evidence than a test, barrel/export, stale runbook string, historical document, or reader of an already-frozen artifact.
- Scope discipline limits the patch, not honest observation. Report a defect reachable through supported inputs or real callers, but do not automatically enlarge the current change for separately owned findings.
- Current source, runtime state, telemetry, and persisted evidence override an old plan. A plan describes intended work, not action-time truth.
- Before proposing a Router, Registry, Manager, Adapter hierarchy, provider abstraction, or new subsystem, test whether the current domain model and owner can satisfy the caller directly.
- At dependency boundaries, prefer the upstream package/service's published exports, declared CLI/bin, schema, or public API over a private build path that merely exists today. Treat private layout as a compatibility edge only when a real supported-version boundary requires it.
- Compare mature alternatives only for a genuinely new architecture pattern, dependency, infrastructure/data system, or substantial custom capability. Do not turn a local fix into open-ended research.

## Decide whether code and complexity should exist

Do not beautify dead code. Before refactoring a complex implementation, prove that a current caller or contract still needs it. If no live owner remains, transfer independently surviving invariants and retire the obsolete implementation rather than packageizing it.

Caller-driven reasoning is transitive: if a caller is removed, merged, deauthorized, or frozen out of execution, recompute dependencies whose survival relied on that caller until no implementation survives solely because of a caller the same change removed.

If an obsolete resolver, registry, alias, or compatibility path has an authoritative-looking name and predictably attracts future changes, prefer deleting or renaming the wrong turn once its surviving obligations are transferred. A warning comment is weaker than removing the invalid choice from the design space.

For a material hotspot, the useful dispositions are:

- **REWRITE** when the current shape obscures or duplicates the real responsibility.
- **RETAIN_JUSTIFIED** when concrete domain, safety, accounting, time/PIT, provider, persistence, concurrency, or other current invariants make the complexity truthful and reviewable.
- **RETIRE** when no current caller or independently surviving invariant pays for the implementation.

Metrics, LOC, branch count, lint counts, and coverage locate areas to inspect; they are not architecture targets.

Treat defensive-looking syntax as a review signal, not a verdict. Retries, fallbacks, locks, hashes, copies, strict modes, compatibility branches, timeouts, and validation earn their place only through the boundary they protect. Before simplifying one, identify:

1. the current supported failure or invariant;
2. whether it is reliably detectable;
3. whether recovery is deterministic enough to trust;
4. whether continuing is materially better than failing clearly;
5. whether the recovery path itself can be verified.

Repeated internal protection of an invariant already guaranteed by a trusted canonical owner is a strong simplification candidate. External, persisted, concurrent, security-sensitive, or irreversible boundaries may legitimately retain explicit protection.

For digests, separate integrity from work avoidance. A hash is useful when a real verifier enforces identity/integrity, or when it avoids a materially more expensive operation and changes the caller's next action. Merely computing or storing a digest is not justification.

## Freeze the shortest durable vertical path

Prefer one route:

`real input -> canonical owner -> domain transformation -> owned side effect/persistence -> observable real output`

Before a non-trivial edit, be able to answer, in working notes or mentally:

`requested outcome -> changed user/system action -> real caller -> current owner -> target owner -> key input/output/side effects -> relevant states -> non-goals/open questions -> expected blast radius -> protected invariants/compatibility -> ordered cutover -> smallest falsifying proof`

This is not a requirement to create a plan document, matrix, Registry, or gate. Explore until material ownership questions are closed, then execute continuously. Replan only when new evidence invalidates an assumption, and state which fact changed which design decision.

Keep one authoritative implementation for each business rule or transformation. DRY applies most strongly to semantics, not repeated syntax. Separate acquisition, domain policy, persistence/external I/O, orchestration, and presentation when they have different owners or change axes; do not split a cohesive responsibility merely to produce more files.

Keep one authoritative owner for each durable fact as well. State, metric, identity, provenance, revision, or behavioral counters should be persisted by the component that uniquely knows them; projections and downstream consumers should not create a second durable truth for convenience.

Prefer explicit data over hidden globals or giant generic `Context`/`Options`/`State` bags. A new helper/module should retire at least one real cognitive burden: duplicate semantic ownership, mixed side effects, repeated traversal/read/parse, giant state plumbing, a real independent change axis, or a substantial presentation block. Cross-file indirection has a cost; extraction that mostly forwards the same context or leaves the old algorithm reachable is not progress.

Define empty, partial, blocked, failed, and complete outcomes where callers distinguish them. At public API/service/protocol boundaries, stable operation/resource identity, request/response shape, status/error semantics, pagination/filtering when applicable, and side-effect/idempotency behavior are part of the contract rather than framework decoration.

Design long-lived process state as bounded state from the start. Recent-history windows, dedupe tables, caches, retry memory, mirrors, and similar structures need a semantic capacity, TTL, or reset condition tied to the behavior they protect; unbounded retention is not the default implementation.

Validate untrusted, persisted, cross-system, permission-sensitive, or irreversible input at its canonical owner. Once that owner establishes a trusted domain contract, do not repeat the same schema/type/hash checks at every internal helper unless a new independent boundary appears.

Compatibility is a budget. Persisted artifacts and external clients may justify compatibility at read/projection/migration edges; dead internal functions used only by tests usually do not. Any temporary compatibility path needs a current owner, scope, stop condition, and removal point.

For optional host/runtime capabilities, distinguish **can run** from **full intended fidelity**. Genuine optional loss may degrade rather than crash, but the consequence and provenance of that degradation should be observable instead of silently masquerading as the complete treatment.

## Structural refactors: semantic seam before physical move

For substantial decomposition or reorganization, use this order unless the task is explicitly path-only:

1. **Module internal** — at the current path, make the responsibility trustworthy: one owner, natural control flow, honest internal types, no confirmed duplicate/dead implementation, and a narrow public surface.
2. **Module connection** — make the handoff explicit: producer identity, meaningful status/empty/failure states, named facts consumed downstream, side-effect/resource ownership, dependency direction. Cut one real caller through the seam, prove the connection, migrate remaining in-scope callers, and retire the old reachable body.
3. **Physical move** — only after the seam is stable, move the cohesive module/family and update imports/exports/path references. Source location, runtime data/config roots, public/persisted identity, and provenance are separate contracts.

Do not move a large tree first and use global import/compiler failures to discover ownership. Do not move a monolith to a prettier directory before its seams are known.

Unexpected blast radius is diagnostic evidence. If a bounded refactor suddenly requires unrelated semantic changes, classify the first divergence before patching further: stale import/path adaptation, hidden real dependency, wrong extraction seam, pre-existing/concurrent failure, or an explicitly authorized cross-cutting migration. Trace from identity/path -> producer input -> canonical owner -> output -> adapter/persistence -> consumer and repair the earliest wrong handoff.

