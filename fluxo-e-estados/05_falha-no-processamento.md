```mermaid
sequenceDiagram
  actor U as Usuário
  participant Web as Front
  participant BFF as BFF
  participant Bus as Pub/Sub
  participant Api as Processamento API
  participant Worker as Processamento Worker
  participant FrioProc as Storage frio Processamento
  participant Quente as Storage quente
  participant Up as Upload
  participant FrioUp as Storage frio Upload
  participant Notif as Notificação
  participant SG as SendGrid

  Note over Worker: vídeo já enfileirado / processando
  Worker->>Worker: falha na extração
  par limpeza no Processamento
    Worker->>FrioProc: remove frames
    Worker->>Quente: remove thumb temporária
  end
  Worker->>Bus: publica falha no processamento
  Bus->>Up: solicita remoção do original
  Up->>FrioUp: remove vídeo original
  Bus->>Notif: consome
  Notif->>SG: e-mail de falha com motivo
  SG-->>U: notificação de falha
  U->>Web: consulta histórico
  Web->>BFF: lista (JWT)
  BFF->>Api: consulta status
  BFF-->>Web: falha + motivo
```
