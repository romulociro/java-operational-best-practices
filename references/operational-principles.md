# Operational Principles for Production Java

This reference consolidates recurring Java operational practices into a neutral working guide for reviews, refactors, and authoring work. Use only the sections relevant to the current task, and confirm version-sensitive or framework-sensitive guidance in `official-sources.md`.

## Exceptions and Failure Contracts

Core guidance:

- Expected domain outcomes should live in return types, not in exceptions.
- Prefer unchecked exceptions when intermediate layers cannot recover meaningfully.
- Exception messages should carry expected versus actual versus operational context.
- Team-wide consistency matters for contract exceptions such as `NullPointerException` versus `IllegalArgumentException`.
- Cross-layer exception translation must preserve the cause.
- Unsafe casts are contract assumptions; validate external types before using them.
- Empty `catch` blocks are ambiguous unless they are explicitly intentional.
- Logging and rethrowing multiplies noise and cost.
- Failure should happen early and with the right exception category.
- Generic `catch (Exception)` hides real bugs and buries evidence.

Review prompts:

- Ask whether the caller can do anything useful with the failure.
- Ask whether the exception carries enough context to diagnose production incidents.
- Ask whether a thrown exception is signaling an invariant failure, a resource failure, or an expected business result.

## Collections, Streams, and Ordering

Core guidance:

- `Collectors.toMap` needs an explicit duplicate-key strategy when duplicates are possible.
- Sequenced Collections standardize first/last and reversed operations, but `reversed()` is a live view.
- Structural mutation during `for-each` is unsafe and can fail partially.

Review prompts:

- If order matters, check whether the chosen collection preserves it explicitly.
- If a transform collects to map form, ask what happens on duplicate keys.
- If code mutates a collection while iterating, look for partial update risks, not just the immediate exception.

## Enums, Contracts, and Type Modeling

Core guidance:

- Closed domain values should be modeled semantically, not as scattered constants.
- Persisting enums by ordinal corrupts data during evolution.
- Using enum `name()` as an API contract creates invisible breaking changes.
- `switch` over enums with `default` removes exhaustiveness protection.
- When the type starts carrying many shape-specific rules, evolve to a closed hierarchy.

Review prompts:

- Distinguish internal implementation names from stable external contract codes.
- Prefer compiler-checked exhaustiveness over fallback defaults.
- If each enum branch carries its own fields and behavior, a sealed hierarchy may fit better.

## Builders, Factories, and Object Creation

Core guidance:

- Static factory methods improve clarity when names explain intent.
- Repeated object creation in hot paths creates unnecessary GC pressure.
- Builders must not leak mutable inputs into supposedly immutable objects.
- Builders should guide construction order when order matters.
- Builders should validate cross-field invariants and avoid silent defaults.

Review prompts:

- If the object is meant to be immutable, verify defensive copies.
- If creation requires business meaning, prefer named factories over ambiguous constructors.
- If the builder is huge, ask whether the type is doing too much or whether staged construction is required.

## Async and Concurrency

Core guidance:

- Avoid blocking async work in the middle of service code.
- Prefer concurrency primitives and patterns that make safe publication obvious.
- In multithreaded random generation, `ThreadLocalRandom` reduces contention.
- External runtime types must be validated explicitly in concurrent and distributed flows.

Review prompts:

- Look for hidden materialization points such as `join()` or `get()` inside non-boundary layers.
- Prefer simpler safe initialization patterns over clever custom locking.
- In distributed code, treat runtime subtype assumptions as unstable until proven otherwise.

## Memory, Lifecycle, and Encapsulation

Core guidance:

- Weak references only solve the intended problem when ownership and reference graphs are correct.
- `try-with-resources` is the default for `AutoCloseable`.
- Public setters and anemic models dissolve invariants across the system.
- Non-static inner helpers can retain outer instances invisibly.

Review prompts:

- Ask what owns the lifecycle of caches, listeners, callbacks, streams, and connections.
- Ask whether object invariants live inside the object or are spread across services.
- Ask whether helper types accidentally retain more state than needed.

## Readability and Local Reasoning

Core guidance:

- Use `var` only when the inferred type is obvious from local reasoning.
- Diagnostic messages should make failures reconstructable without code spelunking.
- Fail-fast structure improves readability by separating invalid paths from the normal path.

Review prompts:

- Prefer local readability over brevity.
- If the type or failure context is not obvious from nearby lines, make it explicit.
