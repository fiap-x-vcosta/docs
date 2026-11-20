# Catálogo de eventos

Contratos de integração via **Google Pub/Sub** ([RNF08](../especificacao/03_requisitos-nao-funcionais.md)–[RNF10](../especificacao/03_requisitos-nao-funcionais.md)). Fonte: [fluxos](../fluxo-e-estados/) e [C4](../arquitetura/). Publicação e consumo: [ADR 003](../adrs/003-outbox-inbox.md).

| # | Evento | Arquivo |
|---|--------|---------|
| 01 | `video-persistido` | [01_video-persistido.md](01_video-persistido.md) |
| 02 | `video-rejeitado-na-triagem` | [02_video-rejeitado-na-triagem.md](02_video-rejeitado-na-triagem.md) |
| 03 | `falha-ao-persistir` | [03_falha-ao-persistir.md](03_falha-ao-persistir.md) |
| 04 | `thumb-disponivel` | [04_thumb-disponivel.md](04_thumb-disponivel.md) |
| 05 | `processamento-sucesso` | [05_processamento-sucesso.md](05_processamento-sucesso.md) |
| 06 | `processamento-falha` | [06_processamento-falha.md](06_processamento-falha.md) |
| 07 | `cancelamento-solicitado` | [07_cancelamento-solicitado.md](07_cancelamento-solicitado.md) |
| 08 | `processamento-cancelado` | [08_processamento-cancelado.md](08_processamento-cancelado.md) |

## Publicador → tópicos

- **Upload:** `video-persistido`, `video-rejeitado-na-triagem`, `falha-ao-persistir`
- **Processamento API:** `cancelamento-solicitado`
- **Processamento Worker:** `thumb-disponivel`, `processamento-sucesso`, `processamento-falha`, `processamento-cancelado`

## Consumidor → tópicos

- **Processamento Worker:** `video-persistido`, `cancelamento-solicitado`
- **Upload:** `thumb-disponivel`, `processamento-sucesso`, `processamento-falha`, `processamento-cancelado`
- **Notificação:** `video-rejeitado-na-triagem`, `falha-ao-persistir`, `processamento-sucesso`, `processamento-falha`
