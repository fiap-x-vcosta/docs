# 01 — `triagem-video-finalizada`

- **Tópico:** `triagem-video-finalizada`
- **Atributo Pub/Sub:** `resultado` = `sucesso` | `falha` (filtro de subscription)
- **Publicador:** Upload
- **Consumidores:** Notificação — filtro `resultado=falha` (sucesso sem consumidor no MVP)
- **Quando:** triagem conclui com aceite ou rejeição

## Envelope

```json
{
  "eventId": "uuid",
  "type": "triagem-video-finalizada",
  "version": 1,
  "occurredAt": "2026-11-20T00:00:00Z",
  "data": {
    "videoId": "uuid",
    "userId": "uuid",
    "resultado": "sucesso"
  }
}
```

```json
{
  "eventId": "uuid",
  "type": "triagem-video-finalizada",
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
