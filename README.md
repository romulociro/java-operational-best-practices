# Java Operational Best Practices

Reusable skill for reviewing, writing, and refactoring production Java code with an emphasis on operational correctness, API contracts, resource safety, and pragmatic modernization.

## What this repository contains

- `SKILL.md`: the entry point used by the agent
- `references/operational-principles.md`: neutral review heuristics for day-to-day Java work
- `references/effective-java-principles.md`: distilled production-oriented guidance aligned with `Effective Java`
- `references/official-sources.md`: official Java, OpenJDK, and framework references for citations and semantic checks

## Focus areas

- Exceptions and failure semantics
- Resource lifecycle and memory retention
- Domain modeling and API contracts
- Collections, streams, async flows, and concurrency
- Builders, factories, and immutability
- Java 21+ modernization when it fits the project baseline

## Design goals

- Stay compatible with the real project baseline instead of forcing the latest Java features
- Prefer operational risk reduction over style churn
- Use neutral guidance rather than content tied to a personal post series
- Cross-check sensitive guidance against official documentation

## Reference sources

- Oracle Java SE 21 API docs
- Java Language Specification
- OpenJDK JEPs and style guides
- Spring Framework reference documentation
- `Effective Java, 3rd Edition`, Joshua Bloch
- `Java Concurrency in Practice`, Brian Goetz et al.

## Local usage

Point the agent at `SKILL.md` and let it load only the relevant reference sections for the current task. The intended reading order is:

1. `SKILL.md`
2. One or more focused sections from `references/operational-principles.md`
3. One or more focused sections from `references/effective-java-principles.md` when design guidance is useful
4. `references/official-sources.md` when citations or semantic verification are needed

## Publishing

This directory is ready to be versioned as its own Git repository. After creating a remote, the typical flow is:

```bash
git init
git add .
git commit -m "Initial version of java-operational-best-practices skill"
git branch -M main
git remote add origin <your-remote-url>
git push -u origin main
```
