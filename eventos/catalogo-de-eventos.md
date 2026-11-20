# Catálogo de eventos

Contratos de integração via **Google Pub/Sub** ([RNF08](../especificacao/03_requisitos-nao-funcionais.md)–[RNF10](../especificacao/03_requisitos-nao-funcionais.md)). Fonte: [fluxos](../fluxo-e-estados/) e [C4](../arquitetura/). Publicação e consumo: [ADR 003](../adrs/003-outbox-inbox.md).

E-mail do destinatário: Notificação resolve via API de Autenticação/`userId` (não via payload).

Tópicos com desfecho binário usam atributo Pub/Sub `resultado` (`sucesso` | `falha`) para filtro de subscription; o `data` é um schema único com campos condicionais ao `resultado`.

| # | Evento | Arquivo |
|---|--------|---------|
| 01 | `persistencia-finalizada` | [01_persistencia-finalizada.md](01_persistencia-finalizada.md) |
| 02 | `video-rejeitado-na-triagem` | [02_video-rejeitado-na-triagem.md](02_video-rejeitado-na-triagem.md) |
| 03 | `thumb-disponivel` | [03_thumb-disponivel.md](03_thumb-disponivel.md) |
| 04 | `processamento-finalizado` | [04_processamento-finalizado.md](04_processamento-finalizado.md) |
| 05 | `cancelamento-solicitado` | [05_cancelamento-solicitado.md](05_cancelamento-solicitado.md) |
| 06 | `processamento-cancelado` | [06_processamento-cancelado.md](06_processamento-cancelado.md) |

## Publicador → tópicos

- **Upload:** `persistencia-finalizada`, `video-rejeitado-na-triagem`
- **Processamento API:** `cancelamento-solicitado`
- **Processamento Worker:** `thumb-disponivel`, `processamento-finalizado`, `processamento-cancelado`

## Consumidor → tópicos

- **Processamento Worker:** `persistencia-finalizada` (`resultado=sucesso`), `cancelamento-solicitado`
- **Upload:** `thumb-disponivel`, `processamento-finalizado`, `processamento-cancelado`
- **Notificação:** `video-rejeitado-na-triagem`, `persistencia-finalizada` (`resultado=falha`), `processamento-finalizado`
