```mermaid
sequenceDiagram
  actor U as Usuário
  participant Web as Front
  participant Bff as bff
  participant Up as upload
  participant FrioUp as Storage frio upload
  participant Quente as Storage quente upload
  participant Bus as Pub/Sub
  participant Worker as processamento-worker
  participant ProcApi as processamento-api
  participant FrioProc as Storage frio processamento
  participant Notif as notificacao
  participant Auth as autenticacao
  participant SG as SendGrid

  U->>Web: envia vídeo
  Web->>Bff: upload (JWT)
  Bff->>Up: encaminha upload
  Up->>Up: triagem OK
  Up->>Bus: publica tópico upload-video-atualizado (triagem/sucesso)
  Up->>FrioUp: grava vídeo original
  Up->>Up: gera URL assinada de leitura
  Up->>Up: status enfileirado
  Up->>Bus: publica tópico upload-video-atualizado (persistencia/sucesso)
  Bus->>Worker: consome tópico upload-video-atualizado (persistencia/sucesso)
  Worker->>FrioUp: GET original via URL assinada
  Worker->>Bus: publica tópico processamento-video-atualizado (iniciado + frameReadUrl)
  Bus->>Up: consome tópico processamento-video-atualizado (iniciado); status processando + thumb
  Up->>Quente: grava thumb
  loop extração de frames
    Worker->>FrioProc: grava frame
  end
  Worker->>FrioProc: grava zip
  par limpeza e status
    Worker->>FrioProc: remove frames
    Worker->>Bus: publica tópico processamento-video-atualizado (sucesso)
  end
  Bus->>Up: consome tópico processamento-video-atualizado (sucesso); remove original
  Up->>FrioUp: remove vídeo original
  Bus->>Notif: consome tópico processamento-video-atualizado (sucesso)
  Notif->>Auth: e-mail por userId
  Notif->>SG: e-mail de sucesso
  SG-->>U: notificação de sucesso
  U->>Web: consulta status / download
  Web->>Bff: lista / URL (JWT)
  Bff->>Up: listagem paginada
  Bff->>ProcApi: URL assinada do zip
  Bff-->>Web: status + URL
  Web-->>U: download do zip
```
