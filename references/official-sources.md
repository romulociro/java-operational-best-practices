# Official Sources

Use these references when a recommendation depends on language or library semantics, or when the user wants a precise source.

## Java Language and Platform

- Java SE 21 API docs: https://docs.oracle.com/en/java/javase/21/docs/api/
- Java Language Specification, SE 21: https://docs.oracle.com/javase/specs/jls/se21/html/index.html
- Java Core Libraries Developer's Guide, SE 21: https://docs.oracle.com/en/java/javase/21/core/java-core-libraries-developer-guide.pdf

## JEPs Relevant to This Skill

- JEP 361, Switch Expressions: https://openjdk.org/jeps/361
- JEP 394, Pattern Matching for `instanceof`: https://openjdk.org/jeps/394
- JEP 395, Records: https://openjdk.org/jeps/395
- JEP 409, Sealed Classes: https://openjdk.org/jeps/409
- JEP 431, Sequenced Collections: https://openjdk.org/jeps/431
- JEP 441, Pattern Matching for `switch`: https://openjdk.org/jeps/441
- JEP 517, HTTP/3 for the HTTP Client API: https://openjdk.org/jeps/517

## Core API Pages Often Needed

- `Throwable`: https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/Throwable.html
- `IllegalArgumentException`: https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/IllegalArgumentException.html
- `NullPointerException`: https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/NullPointerException.html
- `Enum`: https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/Enum.html
- `Optional`: https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Optional.html
- `List.copyOf`: https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/List.html#copyOf(java.util.Collection)
- `Collection.removeIf`: https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Collection.html#removeIf(java.util.function.Predicate)
- `Collectors.toMap`: https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/Collectors.html#toMap(java.util.function.Function,java.util.function.Function)
- `SequencedCollection`: https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/SequencedCollection.html
- `AutoCloseable`: https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/AutoCloseable.html
- `CompletableFuture`: https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/concurrent/CompletableFuture.html
- `ConcurrentHashMap`: https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/concurrent/ConcurrentHashMap.html
- `LongAdder`: https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/concurrent/atomic/LongAdder.html
- `ThreadLocalRandom`: https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/concurrent/ThreadLocalRandom.html
- `WeakHashMap`: https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/WeakHashMap.html
- `Pattern`: https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/regex/Pattern.html
- `DateTimeFormatter`: https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/time/format/DateTimeFormatter.html

## Style and Design Guidance

- Local Variable Type Inference Style Guidelines (`var`): https://openjdk.org/projects/amber/guides/lvti-style-guide

## Framework References

Use these only when the project clearly depends on the framework.

- Spring Framework reference, error responses and `ProblemDetail`: https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-ann-rest-exceptions.html
- Jackson enum serialization/deserialization annotations: https://fasterxml.github.io/jackson-annotations/javadoc/2.17/

## Additional Background References

Use these for broader design guidance, not for language-lawyer semantics.

- Effective Java, 3rd Edition, Joshua Bloch
- Java Concurrency in Practice, Brian Goetz et al.
