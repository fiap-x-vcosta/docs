```mermaid
sequenceDiagram
  actor U as Usuário
  participant Web as Front
  participant BFF as BFF
  participant Api as Processamento API
  participant Bus as Pub/Sub
  participant Worker as Processamento Worker
  participant FrioProc as Storage frio Processamento
  participant Up as Upload
  participant FrioUp as Storage frio Upload
  participant Quente as Storage quente Upload

  Note over U,Api: status enfileirado ou processando
  U->>Web: cancela processamento
  Web->>BFF: cancelar (JWT)
  BFF->>Api: solicita cancelamento
  Api->>Bus: publica cancelamento-solicitado
  Bus->>Worker: entrega comando
  Worker->>Bus: publica processamento-video-status (cancelamento-solicitado)
  Bus->>Up: status cancelamento solicitado
  Worker->>FrioProc: remove frames
  Worker->>Bus: publica processamento-video-status (cancelado)
  Bus->>Up: status cancelado; remove original e thumb
  Up->>FrioUp: remove vídeo original
  Up->>Quente: remove thumb
  Note over Bus: sem notificação por e-mail
  Api-->>BFF: aceito
  BFF-->>Web: confirmado
  Web-->>U: status cancelamento solicitado / cancelado
```
