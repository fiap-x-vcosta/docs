# 05 — `processamento-falha`

- **Tópico:** `processamento-falha`
- **Publicador:** Processamento (Worker)
- **Consumidores:** Upload
- **Quando:** falha na extração/processamento após limpeza de frames

## Envelope

```json
{
  "eventId": "uuid",
  "type": "processamento-falha",
  "version": 1,
  "occurredAt": "2026-11-20T00:00:00Z",
  "data": {
    "videoId": "uuid",
    "userId": "uuid",
    "motivo": "string"
  }
}
```
