# Catálogo de eventos

Contratos de integração via **Google Pub/Sub** ([RNF08](../especificacao/03_requisitos-nao-funcionais.md)–[RNF10](../especificacao/03_requisitos-nao-funcionais.md)). Fonte de comportamento: [fluxos](../fluxo-e-estados/) e [C4](../arquitetura/). Publicação e consumo: [ADR 003](../adrs/003-outbox-inbox.md).

## Convenções

- **Um tópico por `type`** (nome do tópico = `type`).
- **Envelope** (JSON) em toda mensagem:

```json
{
  "eventId": "uuid",
  "type": "video-persistido",
  "version": 1,
  "occurredAt": "2026-11-20T00:00:00Z",
  "data": {}
}
```

- **`eventId`:** UUID gerado pelo publicador; chave de idempotência do consumidor.
- **`version`:** inteiro do contrato deste `type`; breaking change ⇒ novo `version` e novo tópico ou política explícita de dual-publish.
- **Notificação:** o serviço de Notificação **assina os eventos de domínio** que disparam e-mail; não há tópico intermediário de “pedido de notificação”.
- Eventos só internos ao serviço (auth, status local, zip/URL/limpeza de frames antes do publish) **não** entram neste catálogo.

## Eventos

### `video-persistido`

- **Tópico:** `video-persistido`
- **Publicador:** Upload
- **Consumidores:** Processamento (Worker)
- **Quando:** triagem OK e vídeo gravado no frio do Upload
- **`data`:**
  - `videoId` (uuid)
  - `userId` (uuid)
  - `signedReadUrl` (string) — URL assinada de leitura do original ([RNF16](../especificacao/03_requisitos-nao-funcionais.md))
- **Idempotência:** `eventId`; Worker ignora reentrega se o job do `videoId` já existir / já tiver iniciado a partir do mesmo `eventId`

### `video-rejeitado-na-triagem`

- **Tópico:** `video-rejeitado-na-triagem`
- **Publicador:** Upload
- **Consumidores:** Notificação
- **Quando:** triagem rejeita o vídeo
- **`data`:**
  - `videoId` (uuid)
  - `userId` (uuid)
  - `email` (string)
  - `motivo` (string)
- **Idempotência:** `eventId` (um e-mail por `eventId`)

### `falha-ao-persistir`

- **Tópico:** `falha-ao-persistir`
- **Publicador:** Upload
- **Consumidores:** Notificação
- **Quando:** triagem OK e gravação no frio falha
- **`data`:**
  - `videoId` (uuid)
  - `userId` (uuid)
  - `email` (string)
  - `motivo` (string)
- **Idempotência:** `eventId`

### `thumb-disponivel`

- **Tópico:** `thumb-disponivel`
- **Publicador:** Processamento (Worker)
- **Consumidores:** Upload
- **Quando:** há frame utilizável como thumb de catálogo (durante `processando`)
- **`data`:**
  - `videoId` (uuid)
  - `frameReadUrl` (string) — URL assinada de leitura do frame no frio do Processamento
- **Idempotência:** `eventId`; Upload pode substituir a thumb do `videoId` (última escrita vence)

### `processamento-sucesso`

- **Tópico:** `processamento-sucesso`
- **Publicador:** Processamento (Worker)
- **Consumidores:** Upload; Notificação
- **Quando:** zip gravado, frames removidos, status sucesso
- **`data`:**
  - `videoId` (uuid)
  - `userId` (uuid)
  - `email` (string)
  - `zipObjectKey` (string) — referência do zip no frio do Processamento
- **Efeitos:** Upload remove o **vídeo original**; thumb de catálogo **permanece** ([RF19](../especificacao/02_requisitos-funcionais.md)); Notificação envia e-mail de sucesso
- **Idempotência:** `eventId`

### `processamento-falha`

- **Tópico:** `processamento-falha`
- **Publicador:** Processamento (Worker)
- **Consumidores:** Upload; Notificação
- **Quando:** falha na extração/processamento após limpeza de frames
- **`data`:**
  - `videoId` (uuid)
  - `userId` (uuid)
  - `email` (string)
  - `motivo` (string)
- **Efeitos:** Upload remove original e thumb; Notificação envia e-mail de falha com motivo
- **Idempotência:** `eventId`

### `cancelamento-solicitado`

- **Tópico:** `cancelamento-solicitado`
- **Publicador:** Processamento (API)
- **Consumidores:** Processamento (Worker)
- **Quando:** dono cancela com status `enfileirado` ou `processando`
- **`data`:**
  - `videoId` (uuid)
  - `userId` (uuid)
  - `requestedAt` (string, ISO-8601)
- **Idempotência:** `eventId`; Worker no-op se o job já estiver `cancelado`, `sucesso` ou `falha`

### `processamento-cancelado`

- **Tópico:** `processamento-cancelado`
- **Publicador:** Processamento (Worker)
- **Consumidores:** Upload
- **Quando:** Worker concluiu o cancelamento e removeu frames
- **`data`:**
  - `videoId` (uuid)
  - `userId` (uuid)
- **Efeitos:** Upload remove original e thumb; **sem** e-mail ([RF13](../especificacao/02_requisitos-funcionais.md))
- **Idempotência:** `eventId`

## Mapa publicador → tópicos

- **Upload:** `video-persistido`, `video-rejeitado-na-triagem`, `falha-ao-persistir`
- **Processamento API:** `cancelamento-solicitado`
- **Processamento Worker:** `thumb-disponivel`, `processamento-sucesso`, `processamento-falha`, `processamento-cancelado`

## Mapa consumidor → tópicos

- **Processamento Worker:** `video-persistido`, `cancelamento-solicitado`
- **Upload:** `thumb-disponivel`, `processamento-sucesso`, `processamento-falha`, `processamento-cancelado`
- **Notificação:** `video-rejeitado-na-triagem`, `falha-ao-persistir`, `processamento-sucesso`, `processamento-falha`
