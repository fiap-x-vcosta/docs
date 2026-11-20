# 03 — `falha-ao-persistir`

- **Tópico:** `falha-ao-persistir`
- **Publicador:** Upload
- **Consumidores:** Notificação
- **Quando:** triagem OK e gravação no frio falha

## Envelope

```json
{
  "eventId": "uuid",
  "type": "falha-ao-persistir",
  "version": 1,
  "occurredAt": "2026-11-20T00:00:00Z",
  "data": {
    "videoId": "uuid",
    "userId": "uuid",
    "email": "string",
    "motivo": "string"
  }
}
```

- **Idempotência:** `eventId`
