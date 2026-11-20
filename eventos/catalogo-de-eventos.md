# Catálogo de eventos

Contratos de integração via **Google Pub/Sub** ([RNF08](../especificacao/03_requisitos-nao-funcionais.md)–[RNF10](../especificacao/03_requisitos-nao-funcionais.md)). Fonte: [fluxos](../fluxo-e-estados/) e [C4](../arquitetura/). Publicação e consumo: [ADR 003](../adrs/003-outbox-inbox.md).

E-mail do destinatário: Notificação resolve via API de Autenticação/`userId` (não via payload).

Tópicos com desfecho binário usam atributo Pub/Sub `resultado` (`sucesso` | `falha`) para filtro de subscription; o `data` inclui só os campos do desfecho.

| # | Evento | Arquivo |
|---|--------|---------|
| 01 | `triagem-video-finalizada` | [01_triagem-video-finalizada.md](01_triagem-video-finalizada.md) |
| 02 | `persistencia-video-finalizada` | [02_persistencia-video-finalizada.md](02_persistencia-video-finalizada.md) |
| 03 | `thumb-disponivel` | [03_thumb-disponivel.md](03_thumb-disponivel.md) |
| 04 | `processamento-video-finalizado` | [04_processamento-video-finalizado.md](04_processamento-video-finalizado.md) |
| 05 | `cancelamento-solicitado` | [05_cancelamento-solicitado.md](05_cancelamento-solicitado.md) |
| 06 | `processamento-cancelado` | [06_processamento-cancelado.md](06_processamento-cancelado.md) |

## Publicador → tópicos

- **Upload:** `triagem-video-finalizada`, `persistencia-video-finalizada`
- **Processamento API:** `cancelamento-solicitado`
- **Processamento Worker:** `thumb-disponivel`, `processamento-video-finalizado`, `processamento-cancelado`

## Consumidor → tópicos

- **Processamento Worker:** `persistencia-video-finalizada` (`resultado=sucesso`), `cancelamento-solicitado`
- **Upload:** `thumb-disponivel`, `processamento-video-finalizado`, `processamento-cancelado`
- **Notificação:** `triagem-video-finalizada` (`resultado=falha`), `persistencia-video-finalizada` (`resultado=falha`), `processamento-video-finalizado`
