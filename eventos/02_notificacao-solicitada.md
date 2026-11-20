# 02 — `notificacao-solicitada`

- **Tópico:** `notificacao-solicitada`
- **Publicador:** Upload; Processamento (Worker)
- **Consumidores:** Notificação
- **Quando:** ciclo exige e-mail de sucesso ou de falha (rejeição na triagem, falha ao persistir, falha ou sucesso no processamento); cancelamento não publica este evento

## Envelope

```json
{
  "eventId": "uuid",
  "type": "notificacao-solicitada",
  "version": 1,
  "occurredAt": "2026-11-20T00:00:00Z",
  "data": {
    "videoId": "uuid",
    "userId": "uuid",
    "email": "string",
    "tipo": "sucesso | falha",
    "motivo": "string | null"
  }
}
```
