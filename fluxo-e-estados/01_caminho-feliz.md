```mermaid
sequenceDiagram
  actor U as Usuário
  participant Web as Front
  participant BFF as BFF
  participant Up as Upload
  participant FrioUp as Storage frio Upload
  participant Quente as Storage quente Upload
  participant Bus as Pub/Sub
  participant Proc as Processamento
  participant FrioProc as Storage frio Processamento
  participant Notif as Notificação
  participant Auth as Autenticação
  participant SG as SendGrid

  U->>Web: envia vídeo
  Web->>BFF: upload (JWT)
  BFF->>Up: encaminha upload
  Up->>Up: triagem OK
  Up->>Bus: publica triagem-video-finalizada (sucesso)
  Up->>FrioUp: grava vídeo original
  Up->>Up: gera URL assinada de leitura
  Up->>Up: status enfileirado
  Up->>Bus: publica persistencia-video-finalizada (sucesso)
  Bus->>Proc: consome (filtro sucesso)
  Proc->>FrioUp: GET original via URL assinada
  Proc->>Bus: publica processamento-video-status (em-processamento + frameReadUrl)
  Bus->>Up: atualiza status processando + thumb
  Up->>Quente: grava thumb
  loop extração de frames
    Proc->>FrioProc: grava frame
  end
  Proc->>FrioProc: grava zip
  par limpeza e status
    Proc->>FrioProc: remove frames
    Proc->>Bus: publica processamento-video-status (sucesso)
  end
  Bus->>Up: status sucesso; remove original
  Up->>FrioUp: remove vídeo original
  Bus->>Notif: consome (filtro sucesso)
  Notif->>Auth: e-mail por userId
  Notif->>SG: e-mail de sucesso
  SG-->>U: notificação de sucesso
  U->>Web: consulta status / download
  Web->>BFF: lista / URL (JWT)
  BFF->>Up: listagem paginada
  BFF->>Proc: URL assinada do zip
  BFF-->>Web: status + URL
  Web-->>U: download do zip
```
