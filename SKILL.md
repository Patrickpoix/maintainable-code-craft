---
name: maintainable-code-craft
description: A lightweight engineering perspective for implementing, refactoring, reviewing, and simplifying software. Favor whole-system judgment, coherent architecture, natural code, and demonstrable outcomes over rigid workflows, minimal diffs, or ceremonial checks.
---

# Maintainable Code Craft

Build software that is correct, coherent, efficient, and easy for another engineer to understand and change. This skill is a perspective, not a procedure. Use professional judgment; do not turn these principles into required stages, checklists, documents, extra tools, or approvals.

The user's intent and authorization, project instructions, domain contracts, and actual system behavior take precedence. A task may call for a small edit, a substantial redesign, or no change at all. Choose based on the problem, not on a preference for small patches or elaborate architecture.

## Think in systems

Start from the outcome the user needs. Understand enough of the existing system to see how the relevant parts work together: callers, data flow, responsibilities, state, side effects, and the user-visible result. Look beyond the first failing function when the cause or the better solution crosses modules.

Question the system's current shape rather than assuming every existing layer deserves to survive. Identify what is essential, what is accidental, what is duplicated, and what could be simpler. A local fix is right when the problem is local. A coherent cross-module refactor is right when the problem comes from fragmented ownership or a poor boundary.

For broad improvements, form a view of the desired end state before making a string of unrelated micro-optimizations. Reconsider that view as evidence changes. Do not confuse the number of fixes, files touched, tests passed, or lines removed with overall improvement.

## Design for clear ownership

Prefer one understandable source of truth for each meaningful rule or fact. Let the module that owns a responsibility make its decisions; consumers should use its result rather than independently reconstruct it. Distinguish business policy from transport, storage, presentation, and orchestration when that separation removes real coupling.

Use the simplest structure that expresses the system's real responsibilities. Share substantial semantics, not every repeated line. Remove obsolete mechanisms when their callers and obligations are genuinely gone. Avoid speculative managers, registries, adapters, caches, validators, compatibility branches, and second state stores. Equally, do not flatten legitimate boundaries or erase useful behavior merely to make a design look smaller.

Choose module boundaries by what changes independently and who owns the outcome. A long cohesive function can be better than many tiny wrappers; a short function can be wrong if it mixes unrelated decisions. Use mature platform and library capabilities when they solve the actual problem more clearly.

## Write natural code

Prefer straightforward, idiomatic implementation: meaningful names, visible inputs and side effects, direct control flow, appropriately sized interfaces, and explicit failure semantics. Keep context and configuration narrow enough that behavior is understandable without tracing a chain of forwarding helpers.

Treat units, dates, ordering, rounding, missing values, persisted identities, concurrency, and authority as part of a contract whenever they affect correctness. Preserve meaningful distinctions such as missing versus empty, unavailable versus failed, and observed fact versus inference. Do not invent success or conceal an important failure behind a fallback.

Comment non-obvious reasons, constraints, and trade-offs—not syntax that the code already explains. Keep tests and documentation useful, but do not grow permanent engineering machinery to record or verify every routine step.

## Exercise judgment about correctness

A good change solves the intended problem without surprising the real caller. When an interface, parser, default, path, or failure behavior changes, consider the existing observable contract and the inputs most likely to distinguish the old and new behavior. Preserve intentional behavior; change incorrect behavior deliberately rather than accidentally.

Choose evidence that could meaningfully disprove the result. This might be a focused test, a real caller, a representative before/after comparison, or an end-to-end run when the boundary demands it. Cover consequential edge cases, not an exhaustive ritual matrix for every edit. A green test suite is evidence, not a substitute for examining the design and semantic diff.

Optimize measured or clearly material costs without trading away correctness or creating a more complicated system for a negligible gain. For sensitive, irreversible, or domain-critical actions, respect the project's existing guarantees and authorization boundaries. Other specialist methods are optional tools for genuine specialist problems, not mandatory workflow dependencies.

## Judge the whole result

Before considering substantial work complete, look again at the system rather than only at the latest patch. Has responsibility become clearer? Are callers consistently wired to the intended implementation? Did the change introduce a duplicate state, hidden side effect, awkward abstraction, or unexpected behavior? Is the resulting system easier to operate and extend?

Finish the useful scope of the authorized task, remove task-owned dead ends and temporary work when safe, and preserve unrelated files and active services. Do not broaden an analysis into execution or a local change into unrelated work without the user's authorization. For a long, open-ended task, prioritize remaining work by real value and converge toward a coherent finish instead of discovering small improvements indefinitely.

Report what changed, what the available evidence actually establishes, and what remains uncertain. The standard is the quality of the resulting system—not compliance with a process.
