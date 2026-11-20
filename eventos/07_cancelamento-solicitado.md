# 07 — `cancelamento-solicitado`

- **Tópico:** `cancelamento-solicitado`
- **Publicador:** Processamento (API)
- **Consumidores:** Processamento (Worker)
- **Quando:** dono cancela com status `enfileirado` ou `processando`

## Envelope

```json
{
  "eventId": "uuid",
  "type": "cancelamento-solicitado",
  "version": 1,
  "occurredAt": "2026-11-20T00:00:00Z",
  "data": {
    "videoId": "uuid",
    "userId": "uuid",
    "requestedAt": "2026-11-20T00:00:00Z"
  }
}
```

- **Idempotência:** `eventId`; Worker no-op se o job já estiver `cancelado`, `sucesso` ou `falha`
