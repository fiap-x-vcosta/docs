```mermaid
sequenceDiagram
  actor U as Usuário
  participant Web as Front
  participant Bff as bff
  participant Up as upload
  participant FrioUp as Storage frio upload
  participant Bus as Pub/Sub
  participant Notif as notificacao
  participant Auth as autenticacao
  participant SG as SendGrid

  U->>Web: envia vídeo
  Web->>Bff: upload (JWT)
  Bff->>Up: encaminha upload
  Up->>Up: triagem OK
  Up->>Bus: publica tópico upload-video-atualizado (triagem/sucesso)
  Up->>FrioUp: tenta gravar vídeo
  FrioUp-->>Up: falha
  Up->>Up: status falha ao persistir
  Up->>Bus: publica tópico upload-video-atualizado (persistencia/falha)
  Bus->>Notif: consome tópico upload-video-atualizado (persistencia/falha)
  Notif->>Auth: e-mail por userId
  Notif->>SG: e-mail de falha com motivo
  SG-->>U: notificação de falha
  U->>Web: consulta histórico
  Web->>Bff: lista paginada (JWT)
  Bff->>Up: página de vídeos
  Bff-->>Web: falha ao persistir + motivo
```
