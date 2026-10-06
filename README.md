# Java Operational Best Practices

Skill reutilizável para escrever, revisar e refatorar código Java de produção. A fonte primária é o checklist *Pílulas de Java* (Wanderlei Souza); lacunas do corpus são cobertas por referências externas autoritativas — **não inventar** pílulas faltantes.

## O que este repositório contém

O repositório **é o skill**: `SKILL.md` e `references/` ficam na raiz.

| Caminho | Função |
| --- | --- |
| [`SKILL.md`](SKILL.md) | Ponto de entrada do agente (workflow + checklist compacto) |
| [`references/pilulas-checklist.md`](references/pilulas-checklist.md) | Regras numeradas completas por tema |
| [`references/pilulas-corpus.json`](references/pilulas-corpus.json) | Detalhe longo e URL de cada pílula existente |
| [`references/external-refs.md`](references/external-refs.md) | Fontes oficiais quando o tema não está no corpus |

Referências operacionais anteriores, ainda úteis como leitura extra (o skill **não** as carrega no workflow padrão):

- [`references/operational-principles.md`](references/operational-principles.md)
- [`references/effective-java-principles.md`](references/effective-java-principles.md)
- [`references/official-sources.md`](references/official-sources.md)

## Como usar

Aponte o agente para `SKILL.md` e deixe-o carregar **só** as seções relevantes:

1. Classificar o tema (no máximo dois).
2. Aplicar o checklist compacto em `SKILL.md`.
3. Se precisar da regra numerada, ler `references/pilulas-checklist.md` (e `references/pilulas-corpus.json` só para detalhe/URL).
4. Se o tema **não** estiver nas pílulas — inclusive números sem conteúdo: **1–12, 22, 23, 26, 29, 46, 61** — ler `references/external-refs.md` e abrir 1 fonte do tema (máx. 2 se conflitar).

Não carregue todos os `references/` de uma vez.

## Áreas cobertas

- Performance, recursos, IDs e relógio
- API e tipos (`var`, lambdas, generics)
- Collections e streams
- Exceções e logs
- Domínio e design (builders, enums, imutabilidade, JPA)
- Concorrência e Spring
- Testes

## Objetivo

- Preferir mudança mínima e testável
- Tratar correção antes de performance e estilo
- Citar fonte externa (título + URL, ou Item/capítulo de livro pago) quando o corpus não cobrir o tema
- Manter a seção de lacunas como está: não reconstruir pílulas inexistentes
