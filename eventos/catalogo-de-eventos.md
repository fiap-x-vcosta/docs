# Catálogo de eventos

Contratos de integração via **Google Pub/Sub** ([RNF08](../especificacao/03_requisitos-nao-funcionais.md)–[RNF10](../especificacao/03_requisitos-nao-funcionais.md)). Fonte: [fluxos](../fluxo-e-estados/) e [C4](../arquitetura/). Publicação e consumo: [ADR 003](../adrs/003-outbox-inbox.md).

| # | Evento | Arquivo |
|---|--------|---------|
| 01 | `video-persistido` | [01_video-persistido.md](01_video-persistido.md) |
| 02 | `notificacao-solicitada` | [02_notificacao-solicitada.md](02_notificacao-solicitada.md) |
| 03 | `thumb-disponivel` | [03_thumb-disponivel.md](03_thumb-disponivel.md) |
| 04 | `processamento-sucesso` | [04_processamento-sucesso.md](04_processamento-sucesso.md) |
| 05 | `processamento-falha` | [05_processamento-falha.md](05_processamento-falha.md) |
| 06 | `cancelamento-solicitado` | [06_cancelamento-solicitado.md](06_cancelamento-solicitado.md) |
| 07 | `processamento-cancelado` | [07_processamento-cancelado.md](07_processamento-cancelado.md) |

## Publicador → tópicos

- **Upload:** `video-persistido`, `notificacao-solicitada`
- **Processamento API:** `cancelamento-solicitado`
- **Processamento Worker:** `thumb-disponivel`, `processamento-sucesso`, `processamento-falha`, `processamento-cancelado`, `notificacao-solicitada`

## Consumidor → tópicos

- **Processamento Worker:** `video-persistido`, `cancelamento-solicitado`
- **Upload:** `thumb-disponivel`, `processamento-sucesso`, `processamento-falha`, `processamento-cancelado`
- **Notificação:** `notificacao-solicitada`
