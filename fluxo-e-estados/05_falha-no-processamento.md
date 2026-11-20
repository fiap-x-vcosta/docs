```mermaid
sequenceDiagram
  actor U as Usuário
  participant Web as Front
  participant Bff as bff
  participant Bus as Pub/Sub
  participant Worker as processamento-worker
  participant FrioProc as Storage frio processamento
  participant Up as upload
  participant FrioUp as Storage frio upload
  participant Quente as Storage quente upload
  participant Notif as notificacao
  participant Auth as autenticacao
  participant SG as SendGrid

  Note over Worker: vídeo já enfileirado / processando
  Worker->>Worker: falha na extração
  Worker->>FrioProc: remove frames
  Worker->>Bus: publica tópico processamento-video-atualizado (falha)
  Bus->>Up: consome tópico processamento-video-atualizado (falha); remove original e thumb
  Up->>FrioUp: remove vídeo original
  Up->>Quente: remove thumb
  Bus->>Notif: consome tópico processamento-video-atualizado (falha)
  Notif->>Auth: e-mail por userId
  Notif->>SG: e-mail de falha com motivo
  SG-->>U: notificação de falha
  U->>Web: consulta histórico
  Web->>Bff: lista paginada (JWT)
  Bff->>Up: página de vídeos
  Bff-->>Web: falha + motivo
```
