# 08 — `processamento-cancelado`

- **Tópico:** `processamento-cancelado`
- **Publicador:** Processamento (Worker)
- **Consumidores:** Upload
- **Quando:** Worker concluiu o cancelamento e removeu frames

## Envelope

```json
{
  "eventId": "uuid",
  "type": "processamento-cancelado",
  "version": 1,
  "occurredAt": "2026-11-20T00:00:00Z",
  "data": {
    "videoId": "uuid",
    "userId": "uuid"
  }
}
```
