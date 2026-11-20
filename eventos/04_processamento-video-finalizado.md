# 04 — `processamento-video-finalizado`

- **Tópico:** `processamento-video-finalizado`
- **Atributo Pub/Sub:** `resultado` = `sucesso` | `falha` (filtro de subscription)
- **Publicador:** Processamento (Worker)
- **Consumidores:** Upload; Notificação
- **Quando:** processamento termina com sucesso (zip pronto, frames removidos) ou falha (após limpeza de frames)

## Envelope

```json
{
  "eventId": "uuid",
  "type": "processamento-video-finalizado",
  "version": 1,
  "occurredAt": "2026-11-20T00:00:00Z",
  "data": {
    "videoId": "uuid",
    "userId": "uuid",
    "resultado": "sucesso",
    "zipObjectKey": "string"
  }
}
```

```json
{
  "eventId": "uuid",
  "type": "processamento-video-finalizado",
  "version": 1,
  "occurredAt": "2026-11-20T00:00:00Z",
  "data": {
    "videoId": "uuid",
    "userId": "uuid",
    "resultado": "falha",
    "motivo": "string"
  }
}
```
