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

- `signedReadUrl` — URL assinada de leitura do original ([RNF16](../especificacao/03_requisitos-nao-funcionais.md))
- **Idempotência:** `eventId`; Worker ignora reentrega se o job do `videoId` já existir ou já tiver sido iniciado a partir do mesmo `eventId`
