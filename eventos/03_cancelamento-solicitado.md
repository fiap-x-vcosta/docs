# 03 — `cancelamento-solicitado`

- **Tópico:** `cancelamento-solicitado`
- **Escopo:** interno (somente `processamento-api` → `processamento-worker`)
- **Publicador:** `processamento-api`
- **Quando:** `upload` solicita cancelamento a `processamento-api` com status `enfileirado` ou `processando`; `processamento-api` publica no tópico `cancelamento-solicitado`. A projeção `Cancelamento solicitado` em `upload` é gravada no próprio `upload` ao aceitar/enviar o pedido

## Atributos Pub/Sub

```json
[
  {
    "consumidores": ["processamento-worker"]
  }
]
```

## Envelope

```json
{
  "eventId": "uuid",
  "type": "cancelamento-solicitado",
  "version": 1,
  "occurredAt": "2026-11-20T00:00:00Z",
  "data": {
    "videoId": "uuid",
    "userId": "uuid",
    "requestedAt": "2026-11-20T00:00:00Z"
  }
}
```
