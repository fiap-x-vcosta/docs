```mermaid
sequenceDiagram
  actor U as Usuário
  participant Web as Front
  participant BFF as BFF
  participant Up as Upload
  participant FrioUp as Storage frio Upload
  participant Bus as Pub/Sub
  participant Proc as Processamento
  participant FrioProc as Storage frio Processamento
  participant Quente as Storage quente
  participant Notif as Notificação
  participant SG as SendGrid

  U->>Web: envia vídeo
  Web->>BFF: upload (JWT)
  BFF->>Up: encaminha upload
  Up->>Up: triagem OK
  Up->>FrioUp: grava vídeo original
  Up->>Bus: publica vídeo persistido
  Bus->>Proc: consome enfileiramento
  loop extração de frames
    Proc->>FrioProc: grava frame
    Proc->>Quente: atualiza thumb
  end
  Proc->>FrioProc: grava zip
  par limpeza de frames e URL
    Proc->>FrioProc: remove frames
    Proc->>Bus: publica sucesso / zip pronto
  end
  Bus->>Up: solicita remoção do original
  Up->>FrioUp: remove vídeo original
  Bus->>Notif: consome sucesso
  Notif->>SG: e-mail de sucesso
  SG-->>U: notificação de sucesso
  U->>Web: consulta status / download
  Web->>BFF: lista / URL (JWT)
  BFF->>Proc: consulta status
  BFF->>FrioProc: gera URL assinada do zip
  BFF-->>Web: status + URL
  Web-->>U: download do zip
```
