```mermaid
sequenceDiagram
  actor U as Usuário
  participant Web as Front
  participant BFF as BFF
  participant Bus as Pub/Sub
  participant Worker as Processamento Worker
  participant FrioProc as Storage frio Processamento
  participant Up as Upload
  participant FrioUp as Storage frio Upload
  participant Quente as Storage quente Upload
  participant Notif as Notificação
  participant Auth as Autenticação
  participant SG as SendGrid

  Note over Worker: vídeo já enfileirado / processando
  Worker->>Worker: falha na extração
  Worker->>FrioProc: remove frames
  Worker->>Bus: publica processamento-video-status (falha)
  Bus->>Up: status falha; remove original e thumb
  Up->>FrioUp: remove vídeo original
  Up->>Quente: remove thumb
  Bus->>Notif: consome (filtro falha)
  Notif->>Auth: e-mail por userId
  Notif->>SG: e-mail de falha com motivo
  SG-->>U: notificação de falha
  U->>Web: consulta histórico
  Web->>BFF: lista paginada (JWT)
  BFF->>Up: página de vídeos
  BFF-->>Web: falha + motivo
```
