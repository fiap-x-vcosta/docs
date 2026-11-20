# 01 — `video-persistido`

- **Tópico:** `video-persistido`
- **Publicador:** Upload
- **Consumidores:** Processamento (Worker)
- **Quando:** triagem OK e vídeo gravado no frio do Upload

## Envelope

```json
{
  "eventId": "uuid",
  "type": "video-persistido",
  "version": 1,
  "occurredAt": "2026-11-20T00:00:00Z",
  "data": {
    "videoId": "uuid",
    "userId": "uuid",
    "signedReadUrl": "string"
  }
}
```
