# ADR 003: Outbox transacional e inbox idempotente

**Data:** 20 de novembro de 2026  
**Status:** Aceito  
**Autores:** Victor Costa

## 1. Contexto e Problema

Serviços publicam fatos de domínio no Pub/Sub para outros reagirem (processar vídeo, limpar mídia, enviar e-mail). Gravar o estado canônico e publicar a mensagem na mesma operação HTTP, sem coordenação, gera perda ou duplicata. Consumidores pull/streaming no Pub/Sub podem receber a mesma mensagem mais de uma vez ([RNF08](../especificacao/03_requisitos-nao-funcionais.md)–[RNF10](../especificacao/03_requisitos-nao-funcionais.md)).

O problema a ser resolvido é: **Como garantir publicação confiável pós-commit e consumo idempotente entre microsserviços?**

## 2. Decisão

- **Transactional outbox** no PostgreSQL do serviço publicador: na mesma transação do estado de domínio, inserir a linha de outbox com o envelope do [catálogo de eventos](../eventos/catalogo-de-eventos.md) (`eventId`, `type`, `version`, `occurredAt`, `data`).
- **Relay** (hosted service do próprio serviço) lê outbox pendente, publica no tópico Pub/Sub cujo nome é o `type`, e marca a linha como publicada (ou remove). Falha de publish mantém a linha para retry.
- **Inbox** no PostgreSQL de cada consumidor: antes de aplicar o efeito, gravar `eventId` (unique). Se já existir, ack sem reaplicar. Efeito transacional + inbox na mesma transação; para I/O externo (object storage, SendGrid), ação idempotente e inbox após sucesso.
- **Subscriptions** com retry e **DLQ** após esgotar tentativas ([RNF10](../especificacao/03_requisitos-nao-funcionais.md)).
- Publicadores e consumidores **não** compartilham tabelas de outbox/inbox entre serviços.

## 3. Justificativa

- Atende RNF09 (outbox + consumidores idempotentes) sem barramento distribuído de duas fases.
- O `eventId` do catálogo é a chave natural de deduplicação.
- Relay no próprio serviço evita dependência de job externo na janela de demo.

## 4. Alternativas Consideradas

- **Publish direto no request HTTP (sem outbox):** simples; risco de estado commitado sem mensagem (ou o inverso).
- **Change Data Capture (Debezium / Datastream) → Pub/Sub:** desacopla bem; mais peças e tempo de setup para o MVP.
- **Só idempotência no consumidor (sem outbox):** mitiga duplicata; não resolve perda de publish após commit.
- **Inbox compartilhado / store central:** acopla serviços ao mesmo schema; viola posse por domínio.

## 5. Consequências

### Positivas

- Estado canônico e intenção de publish sobem juntos.
- Duplicatas de entrega são tratadas de forma uniforme por `eventId`.
- Cada serviço provisiona apenas o schema que possui.

### Negativas / Riscos (Mitigações)

- **Relay atrasa a entrega sob carga.**
  - *Mitigação:* polling curto; métrica de lag da outbox; alertar se fila crescer na demo.
- **I/O externo (storage/e-mail) fora da transação do inbox.**
  - *Mitigação:* operações idempotentes (mesmo object key / mesmo e-mail por `eventId`); inbox só após sucesso.
- **Ordem entre tópicos distintos não é garantida.**
  - *Mitigação:* consumidores não assumem ordem global; efeitos baseados em `videoId` + status.
