# Catálogo de eventos

Contratos de integração via **Google Pub/Sub** ([RNF08](../especificacao/03_requisitos-nao-funcionais.md)–[RNF10](../especificacao/03_requisitos-nao-funcionais.md)). Fonte: [fluxos](../fluxo-e-estados/) e [C4](../arquitetura/). Publicação e consumo: [ADR 003](../adrs/003-outbox-inbox.md).

E-mail do destinatário: Notificação resolve via API de Autenticação/`userId` (não via payload).

Desfechos binários de triagem/persistência usam atributo `resultado` (`sucesso` | `falha`). Mudanças de status do job de processamento usam atributo `status` no tópico único `processamento-video-status` (sem progresso N/total).

| # | Evento | Arquivo |
|---|--------|---------|
| 01 | `triagem-video-finalizada` | [01_triagem-video-finalizada.md](01_triagem-video-finalizada.md) |
| 02 | `persistencia-video-finalizada` | [02_persistencia-video-finalizada.md](02_persistencia-video-finalizada.md) |
| 03 | `processamento-video-status` | [03_processamento-video-status.md](03_processamento-video-status.md) |
| 04 | `cancelamento-solicitado` | [04_cancelamento-solicitado.md](04_cancelamento-solicitado.md) |

## Publicador → tópicos

- **Upload:** `triagem-video-finalizada`, `persistencia-video-finalizada`
- **Processamento API:** `cancelamento-solicitado`
- **Processamento Worker:** `processamento-video-status`

## Consumidor → tópicos

- **Processamento Worker:** `persistencia-video-finalizada` (`resultado=sucesso`), `cancelamento-solicitado`
- **Upload:** `processamento-video-status`
- **Notificação:** `triagem-video-finalizada` (`resultado=falha`), `persistencia-video-finalizada` (`resultado=falha`), `processamento-video-status` (`status=sucesso` \| `falha`)
