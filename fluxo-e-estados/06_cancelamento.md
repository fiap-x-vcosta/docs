```mermaid
sequenceDiagram
  actor U as Usuário
  participant Web as Front
  participant Bff as bff
  participant Up as upload
  participant ProcApi as processamento-api
  participant Bus as Pub/Sub
  participant Worker as processamento-worker
  participant FrioProc as Storage frio processamento
  participant FrioUp as Storage frio upload
  participant Quente as Storage quente upload

  Note over U,Up: status enfileirado ou processando
  U->>Web: cancela processamento
  Web->>Bff: cancelar (JWT)
  Bff->>Up: solicita cancelamento
  Up->>Up: status cancelamento solicitado
  Up->>ProcApi: solicita cancelamento
  ProcApi->>Bus: publica tópico cancelamento-solicitado
  Bus->>Worker: consome tópico cancelamento-solicitado
  Worker->>FrioProc: remove frames
  Worker->>Bus: publica tópico processamento-video-atualizado (cancelado)
  Bus->>Up: consome tópico processamento-video-atualizado (cancelado); remove original e thumb
  Up->>FrioUp: remove vídeo original
  Up->>Quente: remove thumb
  Note over Bus: sem notificação por e-mail
  Up-->>Bff: aceito
  Bff-->>Web: confirmado
  Web-->>U: status cancelamento solicitado / cancelado
```
