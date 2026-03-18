---
name: java-operational-best-practices
description: Use when reviewing, writing, or refactoring production Java code and the agent must apply project-aware best practices for exceptions, resources, collections, builders, concurrency, async flows, enums, API contracts, or Java 21+ modernization. Trigger on requests like "review this Java code", "refactor this service", "make this idiomatic Java", or "modernize this to Java 21+". Do NOT use for framework-only setup, non-Java languages, or social content about Java.
license: CC-BY-4.0
metadata:
  author: OpenAI Codex
  version: 1.0.0
---

# Java Operational Best Practices

Apply Java best practices in a way that respects the real project instead of forcing generic "clean code" advice. Favor correctness, operability, and compatibility before style churn.

## Instructions

### Step 1: Detect the Project Baseline

Inspect the project before recommending changes.

- Read the build and runtime signals first: `pom.xml`, `build.gradle*`, toolchains, wrapper files, Dockerfiles, CI config, and module descriptors.
- Infer the effective Java baseline from both config and code already in use.
- Detect framework and ecosystem constraints such as Spring Boot, Jakarta, Lombok, JPA, Jackson, Reactor, Quarkus, Micronaut, and internal conventions.
- Treat mixed-version monorepos per module, not as a single global baseline.
- Prefer compatibility with the project's actual baseline over "latest Java" enthusiasm.

Expected output: a short baseline summary before proposing non-trivial changes.

### Step 2: Classify the Task

Decide which mode applies before editing or reviewing.

- `review`: find bugs, operational risks, and maintainability hazards.
- `refactor`: preserve behavior while simplifying contracts or implementation.
- `modernize`: adopt Java 21+ features only when they fit the existing codebase.
- `author`: write new code that matches the project's idioms and constraints.

Load only the relevant sections from `references/operational-principles.md` and `references/effective-java-principles.md`. Cross-check `references/official-sources.md` when you need a citation, version-sensitive behavior, framework semantics, or a source trail for established design guidance.

Expected output: one sentence naming the mode and the highest-risk areas.

### Step 3: Review in Risk Order

Use this order unless the user asks for something narrower.

1. Failure contracts and exceptions
2. Resource lifecycle and memory retention
3. Domain modeling and API contracts
4. Collections, streams, async flow, and concurrency
5. Object creation, builders, and immutability
6. Java 21+ modernization opportunities

Expected output: recommendations ordered by operational impact, not personal taste.

## Core Checklist

### Exceptions and Failure Semantics

- Catch only exceptions the `try` block can really throw. Avoid generic `catch (Exception)`.
- Do not use exceptions for expected domain outcomes. Model expected outcomes in the return type.
- Preserve the original cause when translating exceptions across layers.
- Do not destroy evidence with `new X(e.getMessage())`.
- Avoid `log and throw`. Log once at the boundary that decides retry, fallback, or the final response.
- Empty `catch` blocks are only acceptable when intentionally ignored. Rename the variable to `ignored` and document the condition and effect.
- Exception messages must contain enough context to diagnose the failure without reading source code.
- If the codebase must choose between `NullPointerException` and `IllegalArgumentException` for invalid `null`, enforce consistency with the team standard instead of mixing both.

### Resources and Memory

- Prefer `try-with-resources` for every `AutoCloseable`.
- Flag repeated allocation in hot paths: regex compilation, formatters, object mappers, random generators, and similar reusable objects.
- Eliminate obsolete references in long-lived structures so objects can be reclaimed promptly.
- If a nested helper class does not need outer instance state, make it `static`.
- Be suspicious of long-lived callbacks, listeners, caches, and closures that can retain outer objects.

### Domain Modeling and API Contracts

- Prefer semantic domain types over `static final` constants for closed sets of values.
- Do not persist enums with `ordinal()`.
- Do not expose enum `name()` as an external API contract unless the codebase explicitly treats it as stable.
- Prefer composition over inheritance when reuse would otherwise leak superclass assumptions into the domain.
- Avoid spread-out `switch` logic that "interprets" enums across services.
- When a type discriminator keeps growing, consider a closed hierarchy instead of `enum + switch`.
- Push behavior toward the domain object when invariants depend on object state. Do not normalize an anemic model as "just how Spring works".

### Collections, Streams, Async, and Concurrency

- Never structurally modify a collection inside `for-each`. Use `removeIf` with a pure predicate or explicit iterator removal.
- Treat `reversed()` views in Sequenced Collections as live views, not copies.
- Use `Collectors.toMap` only with an explicit merge strategy when duplicate keys are possible.
- Do not block asynchronous flows in the middle of service code unless the boundary truly requires materialization.
- For multithreaded random generation, prefer `ThreadLocalRandom` unless stronger guarantees are required.
- For lazily initialized shared state, prefer simple safe patterns over fragile hand-rolled concurrency.

### Builders, Factories, and Immutability

- Prefer static factory methods over telescoping constructors when names improve clarity.
- Builders must copy mutable inputs defensively.
- Builders must validate cross-field invariants before object creation completes.
- Treat builders as disposable, not reusable shared state.
- Minimize mutability and verify that value objects keep `equals`, `hashCode`, and `toString` consistent with their contract.
- If Java 21+ records fit the domain, prefer them for immutable carriers with explicit invariants.

### Java 21+ Adaptation Rules

Use modern features only when they fit the project baseline and improve clarity.

- `records`: good for immutable data carriers with compact invariants.
- `sealed` hierarchies: good for closed domains and replacing `switch-on-type`.
- Pattern matching for `instanceof` and `switch`: good when it removes unsafe casts or makes exhaustiveness explicit.
- Sequenced Collections: good when order semantics are central and the project targets Java 21+.
- Do not introduce modern syntax into one file if the module or surrounding code clearly stays conservative, unless the user asked for deliberate modernization.

Expected output: modernization suggestions should include why they help and why they are safe for this codebase.

## Output Style

- For reviews, lead with bugs, risks, and behavioral regressions.
- For refactors, explain what invariant or operational risk the change addresses.
- When a recommendation depends on platform behavior, cite the most relevant official source from `references/official-sources.md`.
- If the project already has a deliberate convention that differs from a generic best practice, follow the project unless the convention is causing a concrete problem.

## Examples

### Example 1: Service Review

User says: "Review this Java service and tell me what's wrong."

Actions:
1. Inspect the build and framework baseline.
2. Check exception handling, resource management, and async flow first.
3. Report findings such as generic catch blocks, `log and throw`, or blocking `join()` in service code.

Result: a review ordered by severity, aligned to the project's Java version and framework conventions.

### Example 2: Enum Refactor

User says: "Refactor this enum mapping to be safer."

Actions:
1. Inspect whether the enum is persisted, serialized, or used as internal-only state.
2. Flag `ordinal()` persistence, unstable `name()` contracts, and spread-out `switch` logic.
3. Recommend stable external codes or behavior on the enum itself, adapting to framework constraints.

Result: a refactor that protects contracts without inventing unnecessary abstraction.

### Example 3: Java 21 Modernization

User says: "Make this Java 21 idiomatic."

Actions:
1. Verify the module really targets Java 21+.
2. Consider records, sealed hierarchies, pattern matching, and Sequenced Collections only where they clarify the code.
3. Preserve interoperability with the rest of the project.

Result: selective modernization that improves readability and safety without style churn.

## Troubleshooting

### Problem: Java version is unclear

Cause: build config, runtime image, and source code signals disagree.

Solution: infer from the narrowest safe baseline used by the module, mention the uncertainty, and avoid version-sensitive syntax until clarified.

### Problem: The project is legacy but partially modernized

Cause: some modules use newer Java features while others remain conservative.

Solution: adapt recommendations per module and avoid cross-cutting rewrites unless explicitly requested.

### Problem: Generated or framework-owned code looks "wrong"

Cause: generated sources and framework adapters often optimize for tooling or contracts, not hand-written style.

Solution: avoid style refactors there unless they cause a real bug, performance issue, or contract problem.
