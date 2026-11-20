# 04 — `processamento-finalizado`

- **Tópico:** `processamento-finalizado`
- **Atributo Pub/Sub:** `resultado` = `sucesso` | `falha` (filtro de subscription)
- **Publicador:** Processamento (Worker)
- **Consumidores:** Upload; Notificação (ambos filtram por `resultado` conforme o efeito)
- **Quando:** processamento termina com sucesso (zip pronto, frames removidos) ou falha (após limpeza de frames)

## Envelope

```json
{
  "eventId": "uuid",
  "type": "processamento-finalizado",
  "version": 1,
  "occurredAt": "2026-11-20T00:00:00Z",
  "data": {
    "videoId": "uuid",
    "userId": "uuid",
    "resultado": "sucesso",
    "zipObjectKey": "string",
    "motivo": null
  }
}
```

```json
{
  "eventId": "uuid",
  "type": "processamento-finalizado",
  "version": 1,
  "occurredAt": "2026-11-20T00:00:00Z",
  "data": {
    "videoId": "uuid",
    "userId": "uuid",
    "resultado": "falha",
    "zipObjectKey": null,
    "motivo": "string"
  }
}
```
