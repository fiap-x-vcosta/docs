# 04 — `thumb-disponivel`

- **Tópico:** `thumb-disponivel`
- **Publicador:** Processamento (Worker)
- **Consumidores:** Upload
- **Quando:** há frame utilizável como thumb de catálogo (durante `processando`)

## Envelope

```json
{
  "eventId": "uuid",
  "type": "thumb-disponivel",
  "version": 1,
  "occurredAt": "2026-11-20T00:00:00Z",
  "data": {
    "videoId": "uuid",
    "frameReadUrl": "string"
  }
}
```
