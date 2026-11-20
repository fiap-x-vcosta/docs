# Catálogo de eventos

Contratos de integração via **Google Pub/Sub** ([RNF08](../especificacao/03_requisitos-nao-funcionais.md)–[RNF10](../especificacao/03_requisitos-nao-funcionais.md)). Fonte: [fluxos](../fluxo-e-estados/) e [C4](../arquitetura/). Publicação e consumo: [ADR 003](../adrs/003-outbox-inbox.md).

E-mail do destinatário: `notificacao` resolve via `autenticacao`/`userId` (não via payload).

Desfechos de triagem/persistência: tópico `upload-video-atualizado` (atributos `etapa` + `resultado`). Ciclo de vida do processamento: tópico `processamento-video-atualizado` (atributo `status`). Consumidores por filtro estão em cada arquivo.

| # | Evento | Escopo | Arquivo |
|---|--------|--------|---------|
| 01 | `upload-video-atualizado` | externo | [01_upload-video-atualizado.md](01_upload-video-atualizado.md) |
| 02 | `processamento-video-atualizado` | externo | [02_processamento-video-atualizado.md](02_processamento-video-atualizado.md) |
| 03 | `cancelamento-solicitado` | interno | [03_cancelamento-solicitado.md](03_cancelamento-solicitado.md) |

## Publicador → tópicos

- **`upload`:** `upload-video-atualizado`
- **`processamento-api`:** `cancelamento-solicitado`
- **`processamento-worker`:** `processamento-video-atualizado`

## Consumidor → tópicos

- **`processamento-worker`:** `upload-video-atualizado` (`etapa=persistencia`, `resultado=sucesso`), `cancelamento-solicitado`
- **`upload`:** `processamento-video-atualizado`
- **`notificacao`:** `upload-video-atualizado` (`resultado=falha`), `processamento-video-atualizado` (`status=sucesso` \| `falha`)
