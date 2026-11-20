# 02 — `processamento-video-atualizado`

- **Tópico:** `processamento-video-atualizado`
- **Escopo:** externo (`upload`; `notificacao`)
- **Publicador:** `processamento-worker`
- **Quando:** `processamento-worker` registra mudança de status do processamento (`iniciado`, `sucesso`, `falha`, `cancelado`)

## Atributos Pub/Sub

```json
[
  {
    "status": "iniciado",
    "consumidores": ["upload"]
  },
  {
    "status": "sucesso",
    "consumidores": ["upload", "notificacao"]
  },
  {
    "status": "falha",
    "consumidores": ["upload", "notificacao"]
  },
  {
    "status": "cancelado",
    "consumidores": ["upload"]
  }
]
```

## Envelope

```json
{
  "eventId": "uuid",
  "type": "processamento-video-atualizado",
  "version": 1,
  "occurredAt": "2026-11-20T00:00:00Z",
  "data": {
    "videoId": "uuid",
    "userId": "uuid",
    "status": "iniciado"
  }
}
```

```json
{
  "eventId": "uuid",
  "type": "processamento-video-atualizado",
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
  "type": "processamento-video-atualizado",
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
  "type": "processamento-video-atualizado",
  "version": 1,
  "occurredAt": "2026-11-20T00:00:00Z",
  "data": {
    "videoId": "uuid",
    "userId": "uuid",
    "status": "cancelado"
  }
}
```
