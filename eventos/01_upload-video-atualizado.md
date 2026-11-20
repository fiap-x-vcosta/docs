# 01 — `upload-video-atualizado`

- **Tópico:** `upload-video-atualizado`
- **Escopo:** externo (`processamento-worker`; `notificacao`)
- **Publicador:** `upload`
- **Quando:** `upload` registra desfecho de triagem ou de persistência do vídeo

## Atributos Pub/Sub

```json
[
  {
    "etapa": "triagem",
    "resultado": "sucesso",
    "consumidores": []
  },
  {
    "etapa": "triagem",
    "resultado": "falha",
    "consumidores": ["notificacao"]
  },
  {
    "etapa": "persistencia",
    "resultado": "sucesso",
    "consumidores": ["processamento-worker"]
  },
  {
    "etapa": "persistencia",
    "resultado": "falha",
    "consumidores": ["notificacao"]
  }
]
```

## Envelope

```json
{
  "eventId": "uuid",
  "type": "upload-video-atualizado",
  "version": 1,
  "occurredAt": "2026-11-20T00:00:00Z",
  "data": {
    "videoId": "uuid",
    "userId": "uuid",
    "etapa": "triagem",
    "resultado": "sucesso"
  }
}
```

```json
{
  "eventId": "uuid",
  "type": "upload-video-atualizado",
  "version": 1,
  "occurredAt": "2026-11-20T00:00:00Z",
  "data": {
    "videoId": "uuid",
    "userId": "uuid",
    "etapa": "triagem",
    "resultado": "falha",
    "motivo": "string"
  }
}
```

```json
{
  "eventId": "uuid",
  "type": "upload-video-atualizado",
  "version": 1,
  "occurredAt": "2026-11-20T00:00:00Z",
  "data": {
    "videoId": "uuid",
    "userId": "uuid",
    "etapa": "persistencia",
    "resultado": "sucesso",
    "signedReadUrl": "string"
  }
}
```

```json
{
  "eventId": "uuid",
  "type": "upload-video-atualizado",
  "version": 1,
  "occurredAt": "2026-11-20T00:00:00Z",
  "data": {
    "videoId": "uuid",
    "userId": "uuid",
    "etapa": "persistencia",
    "resultado": "falha",
    "motivo": "string"
  }
}
```
