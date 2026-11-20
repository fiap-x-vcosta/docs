# 06 — `processamento-falha`

- **Tópico:** `processamento-falha`
- **Publicador:** Processamento (Worker)
- **Consumidores:** Upload; Notificação
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
    "email": "string",
    "motivo": "string"
  }
}
```

- **Efeitos:** Upload remove original e thumb; Notificação envia e-mail de falha com motivo
- **Idempotência:** `eventId`
