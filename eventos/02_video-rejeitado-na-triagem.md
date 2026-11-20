# 02 — `video-rejeitado-na-triagem`

- **Tópico:** `video-rejeitado-na-triagem`
- **Publicador:** Upload
- **Consumidores:** Notificação
- **Quando:** triagem rejeita o vídeo

## Envelope

```json
{
  "eventId": "uuid",
  "type": "video-rejeitado-na-triagem",
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
