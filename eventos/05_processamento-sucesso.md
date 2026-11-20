# 05 — `processamento-sucesso`

- **Tópico:** `processamento-sucesso`
- **Publicador:** Processamento (Worker)
- **Consumidores:** Upload; Notificação
- **Quando:** zip gravado, frames removidos, status sucesso

## Envelope

```json
{
  "eventId": "uuid",
  "type": "processamento-sucesso",
  "version": 1,
  "occurredAt": "2026-11-20T00:00:00Z",
  "data": {
    "videoId": "uuid",
    "userId": "uuid",
    "email": "string",
    "zipObjectKey": "string"
  }
}
```

- `zipObjectKey` — referência do zip no frio do Processamento
- **Efeitos:** Upload remove o **vídeo original**; thumb de catálogo **permanece** ([RF19](../especificacao/02_requisitos-funcionais.md)); Notificação envia e-mail de sucesso
- **Idempotência:** `eventId`
