# 04 — `processamento-sucesso`

- **Tópico:** `processamento-sucesso`
- **Publicador:** Processamento (Worker)
- **Consumidores:** Upload
- **Quando:** zip gravado, frames removidos, status sucesso

## Envelope

```json
{
  "eventId": "uuid",
  "type": "processamento-sucesso",
  "version": 1,
  "occurredAt": "2026-11-20T00:00:00Z",
  "data": {
    "videoId": "uuid",
    "userId": "uuid",
    "zipObjectKey": "string"
  }
}
```
