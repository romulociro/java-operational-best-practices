# Effective Java Aligned Principles

This reference distills production-relevant guidance commonly associated with `Effective Java, 3rd Edition` into review prompts and implementation heuristics. Treat it as supporting guidance for the main skill, not as a substitute for the book or for official Java documentation.

## Object Creation and Lifecycle

Core guidance:

- Prefer static factory methods when names clarify intent, lifecycle, or caching behavior.
- Avoid creating unnecessary temporary objects in hot or frequently executed paths.
- Reuse heavyweight immutable helpers where the lifecycle is obvious and thread-safe.
- Eliminate obsolete references in caches, listeners, collections, and custom pools so the GC can reclaim memory.
- Treat singletons and shared instances as explicit design decisions, not convenient global state.

Review prompts:

- Does this constructor hide meaning that a named factory would reveal?
- Is the code allocating repeatedly inside loops or request paths for objects that could be reused safely?
- Does any long-lived object keep references to data that is no longer needed?

## Classes and Interfaces

Core guidance:

- Minimize mutability unless the domain genuinely needs state transitions.
- Prefer composition over inheritance when reuse would otherwise couple subclasses to fragile implementation details.
- Design and document for inheritance only when the type is intentionally extensible.
- Favor interfaces in APIs and fields when multiple implementations are realistic.
- Minimize accessibility and keep state private unless broader visibility is part of the contract.
- Prefer static member classes when the nested type does not need an enclosing instance.

Review prompts:

- Can this type be immutable or more narrowly mutable?
- Is inheritance being used for convenience instead of a real is-a relationship?
- Are consumers depending on a concrete implementation where an interface would preserve flexibility?

## Methods and API Design

Core guidance:

- Check parameters early and fail with clear, stable contracts.
- Return empty collections or empty option-like results instead of `null` when absence is expected.
- Make defensive copies when accepting or exposing mutable inputs.
- Keep method responsibilities tight so the contract is easy to understand and test.
- Document behavior at the boundary, especially preconditions, postconditions, and failure semantics.

Review prompts:

- Is `null` being used as control flow where an empty result would be clearer?
- Could callers mutate internal state through returned collections or passed-in mutable objects?
- Does the method signature communicate the real contract, or do callers have to infer it from the implementation?

## Equality, Representation, and Value Semantics

Core guidance:

- Value types need consistent `equals` and `hashCode` semantics.
- `toString` should help diagnostics without leaking sensitive data.
- Prefer immutable value carriers for identity-free domain concepts.
- Be cautious with custom comparison logic; ordering should be explicit and stable.

Review prompts:

- If this type participates in sets, maps, or caching, are equality rules explicit and correct?
- Would logs and diagnostics benefit from a better string representation?
- Is this type acting like a value object while still exposing mutable state?

## Enums, Collections, and Generics

Core guidance:

- Prefer enums to ad hoc constants for closed sets of values.
- Use specialized collection types such as `EnumSet` or `EnumMap` when enum keys or flags are central to the model.
- Prefer collections to arrays in public APIs unless a low-level interop boundary requires arrays.
- Keep generics readable; do not trade clarity for cleverness.

Review prompts:

- Is this closed set modeled as raw constants when an enum would be safer?
- Would an enum-specialized collection make intent and performance characteristics clearer?
- Is the public API exposing arrays where collections would be safer and more expressive?

## Pragmatic Use in This Skill

- Prefer the simplest guideline that improves correctness, operability, or maintainability in the current codebase.
- Do not force Bloch-style advice mechanically when the project has a deliberate, working convention.
- When a recommendation is version-sensitive or tied to JDK semantics, confirm it in `official-sources.md`.
