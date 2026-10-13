# Long work and boundary reference

Load this reference only for multi-session/long refactors, runtime-owned checkouts, explicit recovery points, task-owned process cleanup, persisted-data boundaries, or a stop/handoff owned by an external future fact. Consumption-surface proof patterns live in `verification-loop/references/boundary-proof-patterns.md`. Keep ordinary code tasks on the main entry.

## Runtime-owned workspaces and working lifelines

When a scheduler, service, watcher, daemon, or another runtime directly executes the source checkout, treat that checkout as runtime-owned rather than an ordinary edit directory. For non-trivial work, confirm the real executable/source root and choose isolation or an explicit pause/restart plan before editing. Do not impose this ceremony on a checkout no runtime consumes.

Preserve a known working lifeline—remote access, an authenticated session, a production data path, or another currently required route—until evidence shows it must change. Distinguish source from configuration, tests, generated output, caches, runtime/browser state, design experiments, backups, and historical evidence even when they are co-located.

When continuing earlier work, resume from current persisted/worktree truth instead of creating a second implementation from memory.

## Sparse recovery boundary

For a large refactor, migration, or multi-session goal, leave a recovery point only when a coherent responsibility/family has a stable semantic seam, the real caller is cut over, obsolete duplicate ownership is retired unless a real compatibility edge remains, current focused proof supports the implementation claim, unavailable required proof is truthfully `LIMITED/BLOCKED` with a concrete blocker/re-entry condition, the semantic diff has been reviewed, and workspace state is intentionally recoverable.

Keep two states distinct:

- **implementation closed/recoverable** — the code responsibility is coherent and recoverable;
- **acceptance supported/limited/blocked** — external fixtures, data, provider, environment, or other evidence may still gate the final claim.

Do not reopen implementation merely because an external acceptance blocker remains. Re-enter implementation only when fresh evidence invalidates it; when only the blocker changes, re-enter the affected proof.

A Responsibility Unit is a cognitive/semantic/local-proof boundary, not automatically a Git commit, Worklog entry, review round, or full-test boundary. Adjacent units under one owner may share a recovery boundary when they have no independent rollback value. Never use boundary/commit counts as quality metrics, absorb unrelated dirt, checkpoint a knowingly broken half-migration, auto-push, or treat recovery state as final acceptance.

## Cleanup and future-owned facts

Completion includes cleaning task-owned construction residue while preserving unknown/user assets:

- remove task-owned temp files, debug output, stale generated artifacts, disposable screenshots/logs, and temporary workaround/compatibility code when ownership is proven;
- stop task-owned dev servers/watchers/browser drivers/runners through their graceful path and verified identity; never kill by process name, fuzzy command, or stale PID;
- separate representation-only churn from semantic changes so formatting/EOL/generated noise does not hide the actual diff.

If the only remaining gate belongs to wall-clock time, a scheduler, external provider/service, authentication/permission, data arrival, or human approval:

1. record current observed state;
2. record the exact external owner/blocker;
3. record the concrete fact that makes re-entry useful;
4. stop active work rather than polling, sleeping, or inventing adjacent cleanup.

Resume the same authoritative state/workspace when possible. Current disk/runtime truth outranks an old conversation claim. Current/status documents should summarize current truth; historical progression belongs in a Worklog/history surface.

## Persisted data and safety boundaries

Persisted schemas, natural keys, units, timestamps, and field meaning are contracts. Define natural keys before join/aggregation/dedup/upsert. Keep immutable observations separate from explicit latest projections when history and current state have different identities. Never fabricate, silently backfill, or relabel stale/partial/unavailable external data as current or complete.

When cumulative live state is sampled repeatedly into additive or append-only persistence, persist only the unacknowledged delta and advance its baseline only after durable acknowledgement. When a terminal summary or checkpoint depends on effects produced during retirement or settlement, observe it after those final effects settle but before the state carrying them is reset, replaced, or destroyed. Apply these rules only where the store and lifecycle have those semantics; latest-value snapshots and already-delta event streams do not need an extra baseline layer.

Synthetic/mock/sample data belongs in tests, examples, or isolated benchmarks unless project policy explicitly defines another safe use. It cannot substitute for evidence claiming a real-world result.

Hard authorization, destructive-action, security, publication, financial, research/PIT, and project-specific protected-asset semantics remain owned by global/project policy and specialist Skills; this reference does not redefine them.
