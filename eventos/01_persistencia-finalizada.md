# 01 — `persistencia-finalizada`

- **Tópico:** `persistencia-finalizada`
- **Atributo Pub/Sub:** `resultado` = `sucesso` | `falha` (filtro de subscription)
- **Publicador:** Upload
- **Consumidores:** Processamento (Worker) — filtro `resultado=sucesso`; Notificação — filtro `resultado=falha`
- **Quando:** gravação no frio conclui com sucesso ou falha (após triagem OK)

## Envelope

```json
{
  "eventId": "uuid",
  "type": "persistencia-finalizada",
  "version": 1,
  "occurredAt": "2026-11-20T00:00:00Z",
  "data": {
    "videoId": "uuid",
    "userId": "uuid",
    "resultado": "sucesso",
    "signedReadUrl": "string",
    "motivo": null
  }
}
```

```json
{
  "eventId": "uuid",
  "type": "persistencia-finalizada",
  "version": 1,
  "occurredAt": "2026-11-20T00:00:00Z",
  "data": {
    "videoId": "uuid",
    "userId": "uuid",
    "resultado": "falha",
    "signedReadUrl": null,
    "motivo": "string"
  }
}
```
