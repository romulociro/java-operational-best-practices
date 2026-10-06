---
name: Java boas praticas
description: >-
  Use ao escrever, revisar ou refatorar código Java (APIs, collections, streams,
  exceções, domínio, concorrência, Spring, testes) ou ao pedir boas práticas
  Java / Pílulas de Java / Effective Java. Não use para skills genéricas, Python
  ou design de outras linguagens.
---
# Java boas práticas

Aplica boas práticas Java ao implementar, revisar ou refatorar código. Fonte primária: *Pílulas de Java* de Wanderlei Souza. Lacunas: referências externas autoritativas — nunca inventar pílulas faltantes.

## Instruções

### Passo 1: Classificar o trecho

Identifique o tema dominante do código ou do pedido:

- Performance / recursos / IDs / relógio
- API e tipos (var, lambdas, generics leves)
- Collections e streams
- Exceções e logs
- Domínio e design (builders, enums, imutabilidade, JPA)
- Concorrência e Spring
- Testes

Se o pedido misturar temas, escolha no máximo dois e trate em ordem de risco (correção > performance > estilo).

### Passo 2: Carregar só o necessário

1. Use o **checklist compacto** abaixo para o tema escolhido.
2. Se precisar da regra numerada completa ou de mais itens do mesmo tema, leia `references/pilulas-checklist.md` (e `references/pilulas-corpus.json` só se precisar do detalhe/URL).
3. Se o tema **não** estiver coberto pelas pílulas (inclui números sem conteúdo: 1–12, 22, 23, 26, 29, 46, 61), leia `references/external-refs.md` e abra **1 fonte** do tema (máx. 2 se conflitar).
4. Não carregue todos os references de uma vez.

### Passo 3: Aplicar

- Prefira mudança mínima e testável.
- Explique o *porquê* em uma frase quando estiver revisando.
- Não cite a série “Pílulas” a menos que peçam a fonte.
- Ao usar referência externa, cite título + URL (ou Item/capítulo de livro pago, sem colar texto longo).
- Não invente o conteúdo das pílulas faltantes.

### Passo 4: Parar

Entregue o código ou o feedback de review. Não force o checklist inteiro nem abra refs “por curiosidade”.

## Checklist compacto (Pílulas)

### Performance
- #32 Estrutura em memória com teto (fila limitada / limpeza).
- #33 Try-with-resources; AutoCloseable não fecha no GC.
- #40 `nanoTime` para elapsed; `currentTimeMillis`/`Instant` para calendário.
- #41 Preferir UUIDv7/ULID a `UUID.randomUUID()` em índices.
- #50 Inner class → `static` se não usa a outer.
- Sem #: WeakHashMap para cache ligado à chave; não recrie Pattern/ObjectMapper/DateTimeFormatter/Random; ThreadLocalRandom.

### API e tipos
- #25 `var` só com tipo óbvio.
- #37 Value objects imutáveis / records.
- #38 Instantes com ZonedDateTime/Instant; Duration ≠ Period.
- #72 `@Override equals(Object)`; se equals, hashCode.
- #74 `toString` sem grafo JPA/SQL/Locale.
- #77–#78 Wildcards/casts conscientes; validar antes de `@SuppressWarnings`.
- #91–#95 Lambdas curtas; Predicate puro; method ref só se só encaminha; interfaces primitivas; `java.util.function` primeiro.

### Collections e streams
- #14 `toMap` com merge se houver colisão.
- #20 Sequenced: `reversed()` é view.
- #59 Não mute for-each; `removeIf` / `Iterator.remove`.
- #73/#75/#76 equals/hashCode; `List.copyOf`; sem subtrair ints no compare.
- #81–#86 PECS; ParameterizedTypeReference; CAP#1; EnumSet/EnumMap; THC com `Class<T>`.
- #96–#103 `orElseGet`; loop vs stream; filter/map; terminais; groupingBy; limit; flatMap; Gatherers.

### Exceções e logs
- #51–#55 Esperado → retorno tipado; unchecked se só propaga; mensagem reproduzível; cause encadeado.
- #56–#58 instanceof; catch vazio documentado; sem log-and-throw na mesma camada.
- #60 Fail fast; #62 rethrow com `, e`; #66 sem engolir checked no stream.
- #67 `finally` só cleanup; #68 limitar/deduplicar stack de legado; #69 Throwable só em borda.
- #70–#71 Result vs exceção; retry só transitório.

### Domínio e design
- #15–#19 Factories/builders com cópia defensiva e validação.
- #21/#24 Enum > static final; switch expression sem default.
- #30 Utilitária final+static; #31 DI testável; #34 programar para interfaces.
- #36 Record ≠ entity JPA; #39/#42 composição e `final`.
- #43–#49 Contratos úteis; pass-by-value; anti-anêmico.
- #87–#90 Extensão via interface; desserialização segura; crypto AEAD; #88 AOP só para transversal.

### Concorrência e Spring
- #13 Não `join`/`get` no serviço assíncrono.
- #27–#28 Singleton seguro / Spring `@Component`.
- #35 Virtual threads: pin/blocking; #63–#65 HTTP/3, CompletableFuture roles, interrupt.

### Testes
- #16 `@ParameterizedClass`; #45 não public só para testar.
- #79 `List.of` vs `ArrayList` mutável; #80 Datafaker+seed / Instancio.

## Exemplos

### Exemplo 1: Review de stream

Usuário: "revisa esse método que agrupa pedidos"

Ações: tema Collections → checklist #98–#100 → se `toMap` sem merge, aplicar #14 → sugerir `groupingBy` se HashMap manual.

Resultado: diff mínimo + uma frase do porquê.

### Exemplo 2: Feature Spring com auditoria

Usuário: "quero auditar quem aprovou o pagamento"

Ações: #88 marker + Aspect; não misturar Envers/Micrometer; se faltar detalhe de AOP/Spring Security, abrir `references/external-refs.md` (Spring).

Resultado: annotation + aspect esqueleto, sem lógica de negócio dentro do aspect.

### Exemplo 3: Tema fora do corpus

Usuário: "como nomear pacotes neste módulo"

Ações: pílulas não cobrem naming de package → ler `references/external-refs.md` § Naming → Oracle package naming + Code Conventions.

Resultado: recomendação com URL; sem inventar pílula #N.

## Troubleshooting

### Checklist inteiro aplicado de uma vez
Causa: pulou a classificação de tema. Solução: voltar ao Passo 1; no máx. dois temas.

### Quis preencher pílula faltante
Causa: número sem conteúdo (1–12, 22, 23, 26, 29, 46, 61). Solução: usar `external-refs.md`; admitir lacuna se não houver fonte.

### Resposta genérica demais
Causa: não abriu a regra numerada. Solução: carregar `pilulas-checklist.md` para o tema.

## Arquivos de referência

| Arquivo | Quando ler |
| --- | --- |
| `references/pilulas-checklist.md` | Precisa da lista completa por número/tema |
| `references/pilulas-corpus.json` | Precisa de detalhe longo ou URL da pílula |
| `references/external-refs.md` | Tema ausente no corpus ou pílula faltante |
