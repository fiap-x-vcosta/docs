# ADR 002: Progresso canônico e cache no bff

**Data:** 20 de novembro de 2026  
**Status:** Aceito  
**Autores:** Victor Costa

## 1. Contexto e Problema

A listagem precisa mostrar progresso (N/total) enquanto o status é `processando` ([RF14](../especificacao/02_requisitos-funcionais.md)). Redis compartilhado entre `processamento-worker` e `bff` acopla schema de chave e viola database-per-service. Publicar cada frame no Pub/Sub gera ruído e custo.

O problema a ser resolvido é: **Onde fica a verdade do progresso e como o `bff` lê sem banco compartilhado?**

## 2. Decisão

- **Fonte canônica:** PostgreSQL de `processamento` (exposto por `processamento-api`) — contagem de frames feitos / total ([RNF14](../especificacao/03_requisitos-nao-funcionais.md)).
- **`processamento-worker`** grava progresso junto ao checkpoint de controle ([ADR 004](004-processamento-checkpoint-retomada.md)); **não** publica progresso no barramento.
- **`bff`:** cache-aside em **Redis próprio** (compose local / Memorystore dedicado ao `bff` na GCP). Miss → consulta HTTP a `processamento-api`; hit → responde sem ir ao Postgres.
- **Projeção de status** (enfileirado, processando, sucesso, …) permanece em `upload` ([RNF14](../especificacao/03_requisitos-nao-funcionais.md)); o `bff` monta a página pedindo listagem a `upload` e enriquecendo itens `processando` com progresso.
- TTL do cache: curto o bastante para a UX da demo (valor exato na implementação do `bff`).
- Push em tempo real (WebSocket) fica fora do MVP.

## 3. Justificativa

- Mantém um único dono do progresso (`processamento`).
- Redis do `bff` é cache descartável; perda de chave só causa miss.
- Evita tópico de progresso e Memorystore compartilhado entre microsserviços.

## 4. Alternativas Consideradas

- **Redis compartilhado worker↔bff:** latência boa; acopla chave/TTL e quebra posse.
- **Progresso só via Pub/Sub:** desacopla escrita; custo e fan-out desnecessários no MVP.
- **Sem cache no `bff`:** sempre HTTP a `processamento-api`; simples; mais carga sob listagem frequente.

## 5. Consequências

### Positivas

- Contratos claros: status em `upload`, progresso em `processamento-api`.
- Cache local ao gateway sem schema compartilhado.

### Negativas / Riscos (Mitigações)

- **Listagem com muitos itens `processando` multiplica GETs de progresso.**
  - *Mitigação:* cache-aside + batch/timeouts no `bff`; TTL afinado na §11.
- **Cache stale por segundos.**
  - *Mitigação:* aceitável na demo; TTL curto.
