---
name: maintainable-code-craft
description: Use as a general engineering baseline when writing, editing, refactoring, reviewing, or generating code. Prefer clear ownership, direct designs, explicit contracts, proportionate verification, and the simplest implementation that fully solves the current problem. Preserve project-local rules, avoid speculative abstraction, and leave the codebase easier to understand than before.
---

# Maintainable Code Craft

Use this skill as a general maintainability baseline for software work. It is intentionally domain-neutral: project-specific architecture, policy, operational procedures, and business rules belong in the project that owns them.

The goal is not to maximize patterns, layers, checks, or documentation. The goal is to make the smallest complete change that a future maintainer can understand, verify, and safely extend.

## 1. Orient to the current codebase

Before a non-trivial change, establish the current state that matters to the task:

- identify the relevant entrypoint, modules, configuration, tests, and local instructions;
- inspect existing conventions before introducing new ones;
- distinguish source code from generated output, temporary files, and runtime artifacts;
- preserve unrelated user changes;
- prefer current source and executable behavior over stale notes or assumptions.

For a tiny, obvious edit, keep this step tiny as well.

## 2. Trace the real use path

Read code in the direction it is actually used:

`input -> caller -> responsibility -> side effect or result -> observable output`

Do not justify an abstraction merely because a test, export, or old document refers to it. Prefer evidence from supported callers and current behavior.

Before adding a new manager, registry, adapter, router, framework, or generic helper, ask whether the existing design can express the required behavior directly.

## 3. Decide whether complexity earns its place

Every durable abstraction has a maintenance cost. Add one when it removes a real burden such as duplicated semantics, mixed responsibilities, unstable external behavior, or repeated non-trivial work.

Do not add structure solely for hypothetical future flexibility.

Treat complexity metrics as signals, not targets. A long function can be cohesive; a short function can still hide too much indirection. Judge the design by how easily a maintainer can answer:

- What responsibility does this code own?
- Who calls it?
- What inputs and outputs matter?
- What side effects occur?
- What failure states are meaningful?
- Where is the authoritative implementation of the rule?

## 4. Design one clear path

Prefer a direct path from real input to real output.

For a substantial change, form a compact working contract before editing:

- requested outcome;
- relevant caller;
- responsibility being changed;
- important inputs, outputs, and side effects;
- protected behavior or compatibility;
- expected scope;
- smallest useful verification.

This is a reasoning aid, not a requirement to create a design document.

Keep one authoritative implementation for each business rule or transformation. Repetition of syntax is often cheaper than duplicated semantic ownership.

Separate concerns when they truly change independently. Common boundaries include acquisition, domain logic, persistence, orchestration, and presentation. Do not split them mechanically when a single cohesive unit is clearer.

Pass explicit data instead of hiding dependencies in broad mutable context objects or globals. A wrapper that merely forwards the same information through another file is usually not an abstraction improvement.

## 5. Refactor by responsibility, not appearance

Before polishing complicated code, confirm that the responsibility is still needed by a current caller.

For substantial decomposition:

1. make the responsibility understandable at its current location;
2. define a clear interface between producer and consumer;
3. migrate real callers to the new boundary;
4. remove obsolete duplicate ownership;
5. move files only when the semantic boundary is already clear.

Do not move a large tree first and use the resulting errors to discover the architecture.

Unexpected blast radius is diagnostic evidence. When a bounded change suddenly appears to require unrelated edits, identify the first broken assumption before continuing.

## 6. Write natural, reviewable code

Prefer code that is direct, local, predictable, and named after the domain concept it represents.

- Keep each function focused on a coherent responsibility.
- Make side effects visible in names and structure.
- Use guard clauses when they clarify invalid or terminal cases.
- Keep error handling explicit; do not swallow failures or return fake success.
- Follow the repository's language, formatter, linter, and type conventions.
- Reuse the standard library and existing dependencies before adding a package.
- Add a dependency only when it materially reduces current complexity or risk.
- Keep configuration outside source when values vary independently of the code.
- Make time, randomness, units, rounding, and concurrency explicit when they affect correctness.

Do not turn general guidance into mechanical literal elimination or style churn. Protocol constants, enum values, schema fields, and clear test fixtures may remain literal when that is the most readable representation.

## 7. Validate at trust boundaries

Validate data where it first crosses a meaningful trust boundary. After a boundary creates a validated internal representation, trust that contract until another independent boundary appears.

Repeatedly checking the same invariant in every helper usually adds noise without improving safety.

Compatibility should live at the boundary that actually needs it. Do not spread obsolete internal behavior through the core design solely to satisfy tests or historical call shapes.

## 8. Comment for understanding

Structure and naming come before comments.

Use comments and docstrings for information that is difficult to infer from the code itself:

- why a non-obvious decision exists;
- an invariant that must be preserved;
- ordering or failure semantics;
- an external constraint;
- the meaning of a complex calculation or transformation.

Do not translate obvious syntax into prose. Do not use comment-count targets or generated commentary as a substitute for understandable structure.

## 9. Observe the real result

A successful edit or build does not automatically prove that the requested behavior works.

When practical, observe the boundary that the caller or user actually consumes:

- invoke a library through its public contract;
- run a command through its real entrypoint;
- exercise a service through the interface its clients use;
- reload changed persisted data through the real reader;
- inspect an interface in the rendered state that matters.

Choose observation depth according to the claim being made.

## 10. Refine once

After the implementation works, reread the changed responsibility as a maintainer.

Look for:

- duplicated rules;
- unnecessary forwarding layers;
- broad context objects;
- hidden mutable state;
- repeated validation after a trusted boundary;
- speculative fallback paths;
- temporary debugging code;
- stale compatibility branches;
- confusing names or surprising side effects;
- unrelated formatting or generated-file churn.

A refactor succeeds when concepts, ambiguity, or duplicated ownership decrease—not merely when a function becomes shorter.

## 11. Verify the changed claim

Verification should be proportionate to the change.

- Run focused checks that can falsify the intended behavior.
- Add regression coverage when the behavior is important, repeatable, and not already protected.
- Reuse unchanged evidence only when the relevant code and environment truly did not change.
- Never describe an unrun test, build, audit, or manual check as passed.

Prefer a small number of meaningful checks over broad ritual testing unrelated to the change.

## 12. Leave the workspace deliberate

Before finishing:

- review the final diff;
- preserve unrelated changes;
- remove task-owned temporary files and debugging residue;
- remove workarounds that are no longer needed;
- keep generated output and runtime artifacts out of source locations unless the project deliberately owns them there;
- stop task-owned auxiliary processes through their normal lifecycle when appropriate.

Do not delete unknown assets merely to make the tree look cleaner.

## 13. Stop at the correct boundary

A task may end because the change is complete, because a local failure is understood, or because the remaining step is owned by an external dependency or human decision.

When further progress depends on something outside the current execution context, record the current state, the blocker, and the condition that would make re-entry useful. Do not invent unrelated work merely to stay busy.

## Data and safety baseline

- Treat persisted schemas, identifiers, units, timestamps, and field meaning as contracts.
- Define stable keys before joins, aggregation, deduplication, or updates.
- Keep historical observations distinct from current projections when their identities differ.
- Do not present stale, partial, synthetic, or unavailable data as complete real-world evidence.
- Keep secrets, credentials, private records, and environment-specific values out of source and logs.
- Guard destructive or irreversible actions at the actual mutation boundary required by the project.
- Keep project-specific safety, release, data, and domain rules in project-local instructions rather than generalizing them into this skill.

## Scale the workflow

For a small obvious fix, the full cycle may be only:

`understand -> change -> inspect -> verify -> clean`

For a larger refactor or migration, spend more effort on ownership, compatibility, observation, and recovery. Increase process only when risk and scope justify it.

## Completion standard

Before claiming completion, confirm that:

- the requested outcome is actually addressed;
- the real caller reaches one clear implementation;
- new complexity has a present justification;
- important behavior was observed when practical;
- applicable verification actually ran;
- temporary task residue is resolved;
- unrelated work was preserved;
- remaining blockers are stated accurately.