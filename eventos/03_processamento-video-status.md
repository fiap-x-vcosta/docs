# 03 — `processamento-video-status`

- **Tópico:** `processamento-video-status`
- **Atributo Pub/Sub:** `status` = `em-processamento` | `cancelamento-solicitado` | `sucesso` | `falha` | `cancelado` (filtro de subscription)
- **Publicador:** Processamento (Worker) — único publicador
- **Consumidores:** Upload; Notificação — filtros `status=sucesso` e `status=falha`
- **Quando:** o Worker registra mudança de status do job (não publica progresso N/total)

## Envelope

```json
{
  "eventId": "uuid",
  "type": "processamento-video-status",
  "version": 1,
  "occurredAt": "2026-11-20T00:00:00Z",
  "data": {
    "videoId": "uuid",
    "userId": "uuid",
    "status": "em-processamento",
    "frameReadUrl": "string"
  }
}
```

```json
{
  "eventId": "uuid",
  "type": "processamento-video-status",
  "version": 1,
  "occurredAt": "2026-11-20T00:00:00Z",
  "data": {
    "videoId": "uuid",
    "userId": "uuid",
    "status": "cancelamento-solicitado"
  }
}
```

```json
{
  "eventId": "uuid",
  "type": "processamento-video-status",
  "version": 1,
  "occurredAt": "2026-11-20T00:00:00Z",
  "data": {
    "videoId": "uuid",
    "userId": "uuid",
    "status": "sucesso",
    "zipObjectKey": "string"
  }
}
```

```json
{
  "eventId": "uuid",
  "type": "processamento-video-status",
  "version": 1,
  "occurredAt": "2026-11-20T00:00:00Z",
  "data": {
    "videoId": "uuid",
    "userId": "uuid",
    "status": "falha",
    "motivo": "string"
  }
}
```

```json
{
  "eventId": "uuid",
  "type": "processamento-video-status",
  "version": 1,
  "occurredAt": "2026-11-20T00:00:00Z",
  "data": {
    "videoId": "uuid",
    "userId": "uuid",
    "status": "cancelado"
  }
}
```
