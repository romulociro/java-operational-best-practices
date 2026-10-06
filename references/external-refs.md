# Referências externas de Java (lacunas do corpus Pílulas)

> **Propósito:** fontes autoritativas públicas para um agent skill de best practices Java quando o checklist **Pílulas de Java** (Wanderlei Souza / wandi) não cobre o tema.
>
> **Corpus interno:** 85 pílulas numeradas existentes.  
> **Pílulas sem conteúdo (não inventar):** 1–12, 22, 23, 26, 29, 46, 61.  
> **Regra:** este arquivo **não** reconstrói o que essas pílulas diziam. Use só as fontes listadas abaixo (ou o corpus existente quando houver conteúdo).

**Como o agente deve usar este arquivo**

1. Preferir o corpus Pílulas quando o número/tema já estiver coberto.
2. Abrir **1 fonte** da lista do tema (máx. 2 se conflitar ou faltar detalhe).
3. Citar título + URL ao orientar; não inventar itens de livros sem consultar a obra.
4. Livros pagos (Effective Java, JCIP): listar Items/capítulos por número; não colar texto longo.

*Formato inspirado em skill-architect `references/` (carregar só quando o tema exigir).*

---

## 1. Naming, packages, API design

### Code Conventions for the Java Programming Language — §9 Naming Conventions
- **URL:** https://www.oracle.com/java/technologies/javase/codeconventions-namingconventions.html
- **Por quê:** convenções clássicas Oracle/Sun (pacotes, classes, métodos, constantes).
- **Quando abrir:** dúvida de camelCase/UPPER_SNAKE, nomes de constantes, ou review de estilo de nomes.

### Naming a Package (The Java™ Tutorials)
- **URL:** https://docs.oracle.com/javase/tutorial/java/package/namingpkgs.html
- **Por quê:** regra oficial de pacotes (`com.example…`, lowercase, domínio invertido).
- **Quando abrir:** criar/renomear pacotes, módulos, ou layout de projeto.

### How to Design a Good API and Why it Matters (Joshua Bloch) — InfoQ / ACM
- **URL (InfoQ):** https://www.infoq.com/presentations/effective-api-design/
- **Por quê:** princípios amplamente citados de desenho de API (consistência, nomes como linguagem pequena, simetria).
- **Quando abrir:** desenhar API pública de biblioteca/módulo; revisar assinaturas e nomes de métodos além do estilo superficial.
- **Nota:** apresentação/vídeo; complementar com Effective Java Items 15–22, 64, 68 (ver §9).

---

## 2. Equality, hashCode, Comparable (além do corpus)

### java.lang.Object — equals / hashCode (Java SE API)
- **URL:** https://docs.oracle.com/en/java/javase/26/docs/api/java.base/java/lang/Object.html
- **Por quê:** contrato oficial (reflexivo, simétrico, transitivo, consistente; `hashCode` coerente com `equals`).
- **Quando abrir:** implementar ou auditar `equals`/`hashCode`; bugs em `HashMap`/`HashSet`.

### java.lang.Comparable (Java SE API)
- **URL:** https://docs.oracle.com/en/java/javase/26/docs/api/java.base/java/lang/Comparable.html
- **Por quê:** contrato de ordem total; recomendação de consistência `compareTo` ↔ `equals`.
- **Quando abrir:** `TreeSet`/`TreeMap`, ordenação natural, inconsistências equals/compareTo.

### Effective Java (Bloch), 3ª ed. — Items 10, 11, 12, 14
- **Livro (pago):** *Effective Java*, 3rd Edition — Joshua Bloch
- **Por quê:** tratamento canônico de equals/hashCode/toString/Comparable com armadilhas e padrões.
- **Quando abrir:** além do Javadoc; herança + equals, campos mutáveis, records vs classes.
- **Itens:** 10 (equals), 11 (hashCode), 12 (toString), 14 (Comparable).

---

## 3. Generics / type erasure / PECS (deep dive)

### Wildcards (The Java™ Tutorials — Generics)
- **URL:** https://docs.oracle.com/javase/tutorial/java/generics/wildcards.html
- **Por quê:** tutorial oficial; PECS (*Producer Extends, Consumer Super*) com `? extends` / `? super`.
- **Quando abrir:** APIs com wildcards, `List<? extends T>`, consumidores/produtores genéricos.

### Type Erasure (The Java™ Tutorials — Generics)
- **URL:** https://docs.oracle.com/javase/tutorial/java/generics/erasure.html
- **Por quê:** explica erasure, restrições em runtime, bridges.
- **Quando abrir:** `ClassCastException` “misterioso”, `instanceof` com tipo paramétrico, arrays vs listas genéricas.

### Effective Java (Bloch), 3ª ed. — Items 26–33
- **Livro (pago):** *Effective Java*, 3rd Edition
- **Por quê:** raw types, unchecked warnings, lists vs arrays, bounded wildcards, varargs + generics, heterogeneous containers.
- **Quando abrir:** design de API genérica, silenciar warnings com critério, PECS em bibliotecas.
- **Itens-chave:** 26, 27, 28, 29, 30, 31 (PECS), 32, 33.

---

## 4. Concurrency (JCIP + virtual threads Oracle)

### Java Concurrency in Practice (Goetz et al.)
- **Livro (pago):** *Java Concurrency in Practice* — Brian Goetz et al. (Addison-Wesley)
- **Por quê:** referência clássica de thread safety, publicação segura, locks, executors (ainda válida; virtual threads complementam).
- **Quando abrir:** shared mutable state, visibility, deadlocks, documentar thread safety.
- **Capítulos úteis (orientação):** Part I (fundamentos / thread safety); Cap. 5–8 (building blocks, tasks, cancellation); Cap. 11–13 (performance, testing, locks) — consultar índice da edição em mãos.
- **Nota JCIP citada pela plataforma:** JEP 491 reforça JCIP §13.4 — preferir `synchronized` quando prático; `ReentrantLock` quando precisar de flexibilidade.

### Virtual Threads (Oracle Java SE Docs) + JEP 444
- **URL (docs):** https://docs.oracle.com/en/java/javase/23/core/virtual-threads.html  
  (atualizar major version no path conforme JDK do projeto, ex. 21+)
- **URL (JEP):** https://openjdk.org/jeps/444
- **Por quê:** modelo oficial de virtual threads (carrier, blocking I/O, `newVirtualThreadPerTaskExecutor`).
- **Quando abrir:** throughput I/O-bound, migrar de pools de platform threads, pinning/`synchronized` (ver JEP 491: https://openjdk.org/jeps/491).

### Effective Java (Bloch), 3ª ed. — Items 78–84
- **Quando abrir:** checklist rápido de concorrência no dia a dia (sync, executors, documentar thread safety).
- **Itens:** 78–84.

---

## 5. Memory / GC / escape analysis (alto nível)

### HotSpot Virtual Machine Garbage Collection Tuning Guide (Oracle)
- **URL:** https://docs.oracle.com/en/java/javase/26/gctuning/
- **Por quê:** guia oficial dos coletores (G1, ZGC, Parallel, etc.) e fatores de performance.
- **Quando abrir:** latência/pausa de GC, escolha de coletor, sizing de heap — **não** como micro-otimização prematura.

### Java HotSpot Virtual Machine Performance Enhancements (escape analysis)
- **URL:** https://docs.oracle.com/en/java/javase/24/vm/java-hotspot-virtual-machine-performance-enhancements.html
- **Por quê:** visão oficial de melhorias HotSpot, incluindo escape analysis / scalar replacement (alto nível).
- **Quando abrir:** explicar por que alocações “sumem”, locks em objetos não-escapantes, ou discutir otimizações da JVM sem inventar mitos de “stack allocation”.

### EscapeAnalysis (OpenJDK HotSpot Wiki) — opcional/aprofundamento
- **URL:** https://wiki.openjdk.org/display/HotSpot/EscapeAnalysis
- **Por quê:** detalhes de implementação (GlobalEscape / ArgEscape / NoEscape).
- **Quando abrir:** só se o usuário pedir profundidade de compilador/JIT; manter resposta de produto no Tuning Guide + Performance Enhancements.

---

## 6. Secure coding (deserialization, crypto)

### Secure Coding Guidelines for Java SE (Oracle)
- **URL:** https://www.oracle.com/java/technologies/javase/seccodeguide.html
- **Por quê:** guia oficial Oracle (privilégio mínimo, serialização, etc.).
- **Quando abrir:** review de segurança geral em código Java SE; baseline antes de regras CERT específicas.

### Java Serialization Filters + Addressing Deserialization Vulnerabilities (Oracle)
- **URL (filters):** https://docs.oracle.com/en/java/javase/27/core/java-serialization-filters.html  
  (ajustar versão JDK do path)
- **URL (vulnerabilities):** https://docs.oracle.com/en/java/javase/27/core/addressing-serialization-vulnerabilities.html
- **Por quê:** `ObjectInputFilter`, `jdk.serialFilter`, JEP 290/415 — prática moderna contra gadget chains.
- **Quando abrir:** qualquer `ObjectInputStream` / dados serializados de fora do trust boundary.

### SEI CERT Oracle Coding Standard for Java — Serialization (SER)
- **URL (índice SER):** https://cmu-sei.github.io/secure-coding-standards/sei-cert-oracle-coding-standard-for-java/rules/serialization-ser/
- **Destaques:** SER12-J (não deserializar dados não confiáveis); SER02-J (sign then seal); evitar crypto caseira.
- **Quando abrir:** regras normativas com exemplos compliant/noncompliant; auditoria de serialização/crypto.

---

## 7. Testing (JUnit 5/6, Testcontainers)

### JUnit User Guide (JUnit 6 — atual; JUnit 5 ainda comum em projetos legados)
- **URL (JUnit 6):** https://docs.junit.org/6.1.3/user-guide/  
  (ou https://docs.junit.org/ — seguir versão do projeto)
- **URL (JUnit 5.x legado):** https://docs.junit.org/5.14.0/user-guide/
- **Por quê:** documentação oficial Jupiter/Platform (lifecycle, extensions, parameterized, parallel).
- **Quando abrir:** escrever/migrar testes, extensions, diferenças 5→6.

### Testcontainers for Java
- **URL:** https://java.testcontainers.org/
- **Integração JUnit 5/Jupiter:** https://java.testcontainers.org/test_framework_integration/junit_5/
- **Por quê:** containers reais (DB, brokers) em testes de integração.
- **Quando abrir:** testes com Postgres/Kafka/Redis reais; `@Testcontainers` / `@Container`; singleton vs per-test.

---

## 8. Spring Boot practices (injection, transactions, testing)

### Spring Beans and Dependency Injection (Spring Boot Reference)
- **URL:** https://docs.spring.io/spring-boot/reference/using/spring-beans-and-dependency-injection.html
- **Por quê:** como o Boot registra beans e injeta dependências no estilo idiomático.
- **Quando abrir:** constructor injection vs field injection, configuração de beans, conflitos de wiring.

### Using @Transactional (Spring Framework)
- **URL:** https://docs.spring.io/spring/reference/6.2/data-access/transaction/declarative/annotations.html
- **Por quê:** comportamento de proxy, self-invocation, rollback rules, `readOnly`.
- **Quando abrir:** transações que “não abrem”, rollback inesperado, chamadas internas na mesma classe.

### Testing Spring Boot Applications (Spring Boot Reference)
- **URL:** https://docs.spring.io/spring-boot/reference/testing/spring-boot-applications.html
- **Complemento (transações em testes):** https://docs.spring.io/spring/reference/6.2/testing/testcontext-framework/tx.html
- **Por quê:** `@SpringBootTest`, slices (`@WebMvcTest`, etc.), rollback padrão em testes `@Transactional`.
- **Quando abrir:** estratégia de teste Boot (unit vs slice vs full context); mocks (`@MockitoBean`).

---

## 9. Effective Java (Bloch) — Items mais relevantes no dia a dia

**Obra:** *Effective Java*, 3rd Edition — Joshua Bloch (Addison-Wesley / Pearson).  
**URL informativa Oracle (página do livro):** https://www.oracle.com/java/technologies/effectivejava.html  
**Uso:** livro pago — o agente cita **números de Item**, não reimprime o texto.

### Checklist prioritário (código cotidiano)

| Item | Tema |
|------|------|
| **5** | Prefer dependency injection to hardwiring resources |
| **6** | Avoid creating unnecessary objects |
| **7** | Eliminate obsolete object references |
| **9** | Prefer try-with-resources to try-finally |
| **10** | Obey the general contract when overriding equals |
| **11** | Always override hashCode when you override equals |
| **12** | Always override toString |
| **14** | Consider implementing Comparable |
| **15** | Minimize the accessibility of classes and members |
| **17** | Minimize mutability |
| **18** | Favor composition over inheritance |
| **19** | Design and document for inheritance or else prohibit it |
| **20** | Prefer interfaces to abstract classes |
| **26** | Don’t use raw types |
| **27** | Eliminate unchecked warnings |
| **28** | Prefer lists to arrays |
| **29–31** | Favor generic types/methods; bounded wildcards (PECS) |
| **34** | Use enums instead of int constants |
| **40** | Consistently use the Override annotation |
| **49** | Check parameters for validity |
| **50** | Make defensive copies when needed |
| **52** | Refer to objects by their interfaces *(relacionado a 64)* |
| **57** | Minimize the scope of local variables |
| **58** | Prefer for-each loops to traditional for loops |
| **59** | Know and use the libraries |
| **61** | Prefer primitive types to boxed primitives |
| **64** | Refer to objects by their interfaces |
| **65** | Prefer interfaces to reflection |
| **67** | Optimize judiciously |
| **68** | Adhere to generally accepted naming conventions |
| **78–82** | Concurrency: sync, executors, utilities, document thread safety |

**Quando abrir o livro:** decisão de design não coberta pelas Pílulas; o agente aponta o **Item N** e resume a regra em uma frase própria (sem copiar prosa longa da obra).

---

## 10. Official style: Google Java Style Guide, OpenJDK LVTI/var

### Google Java Style Guide
- **URL:** https://google.github.io/styleguide/javaguide.html
- **Por quê:** estilo amplamente adotado (formatação, nomes, imports, Javadoc).
- **Quando abrir:** formatação/nomes em PRs; alinhar projeto sem style guide próprio; type variables (§5.2.8).

### Local Variable Type Inference: Style Guidelines (OpenJDK Amber)
- **URL:** https://openjdk.org/projects/amber/guides/lvti-style-guide
- **FAQ:** https://openjdk.org/projects/amber/guides/lvti-faq
- **Docs linguagem (Oracle):** https://docs.oracle.com/en/java/javase/26/language/local-variable-type-inference.html
- **Por quê:** diretrizes oficiais G1–G7 para `var` (nomes informativos, escopo mínimo, diamond/generics).
- **Quando abrir:** review de `var`; quando o tipo fica obscuro; conflito “programming to the interface” vs LVTI.

---

## Índice rápido: tema → abrir primeiro

| Tema | Abrir primeiro |
|------|----------------|
| Nomes / pacotes | Oracle Naming Conventions + Naming a Package |
| API pública | Bloch API talk + EJ Items 15–22, 64, 68 |
| equals / hashCode / Comparable | Javadoc Object/Comparable → EJ 10–14 |
| Generics / PECS / erasure | Tutorials Wildcards + Type Erasure → EJ 26–33 |
| Concorrência clássica | JCIP (+ EJ 78–84) |
| Virtual threads | Oracle Virtual Threads + JEP 444 |
| GC / memória | GC Tuning Guide (alto nível) |
| Escape analysis | HotSpot Performance Enhancements |
| Deserialização / crypto | Oracle Secure Coding + Serialization Filters; CERT SER |
| Testes unitários | JUnit User Guide (versão do projeto) |
| Testes com infra | Testcontainers + JUnit Jupiter module |
| Spring DI / TX / testes | Spring Boot DI + `@Transactional` + Testing Boot Apps |
| Estilo / `var` | Google Java Style + OpenJDK LVTI Style Guide |

---

## Lacunas do corpus (lembrete operacional)

| Números sem conteúdo | Ação do agente |
|----------------------|----------------|
| 1–12, 22, 23, 26, 29, 46, 61 | **Não inventar** o texto da pílula. Usar este arquivo + corpus existente nos demais números. |

---

## Metadados

- **Gerado para:** agent skill de Java best practices (complemento às Pílulas de Java / wandi).
- **Fontes preferidas:** docs Oracle/OpenJDK, Spring, JUnit, Testcontainers, SEI CERT; livros citados por Item/capítulo.
- **Não incluir:** tip inventado atribuído a Wandi; paste longo de livros pagos; exploits ou bypass de segurança.
