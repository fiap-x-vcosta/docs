```mermaid
sequenceDiagram
  actor U as Usuário
  participant Web as Front
  participant Bff as bff
  participant Redis as Redis bff
  participant Bus as Pub/Sub
  participant Up as upload
  participant Quente as Storage quente upload
  participant ProcApi as processamento-api
  participant Worker as processamento-worker
  participant PGP as Postgres processamento
  participant FrioUp as Storage frio upload
  participant FrioProc as Storage frio processamento

  Note over Bus,Worker: tópico upload-video-atualizado (persistencia/sucesso)
  Bus->>Worker: consome tópico upload-video-atualizado (persistencia/sucesso)
  Worker->>FrioUp: GET original via URL assinada
  Worker->>PGP: status processando + total estimado + checkpoint
  Worker->>Worker: FFmpeg extrai primeiro frame
  Worker->>FrioProc: grava frame
  Worker->>PGP: progresso + checkpoint
  Worker->>Bus: publica tópico processamento-video-atualizado (iniciado)
  Bus->>Up: consome tópico processamento-video-atualizado (iniciado); status processando
  Up->>ProcApi: GET frame válido (thumb)
  ProcApi-->>Up: bytes / URL de leitura do frame
  Up->>Quente: grava thumb

  loop demais amostras (1 a cada 30 frames da fonte)
    Worker->>Worker: FFmpeg extrai frame
    Worker->>FrioProc: grava frame
    Worker->>PGP: progresso N/total + checkpoint
  end

  U->>Web: consulta listagem
  Web->>Bff: GET lista (JWT, página)
  Bff->>Up: página de vídeos (projeção de status + thumb)
  alt item processando
    Bff->>Redis: busca progresso em cache
    alt cache miss
      Bff->>ProcApi: progresso N/total
      ProcApi->>PGP: lê progresso canônico
      ProcApi-->>Bff: N/total
      Bff->>Redis: grava cache-aside
    end
  end
  Bff-->>Web: itens + status + progresso opcional + thumb
  Web-->>U: lista

  Worker->>FrioProc: grava zip
  Worker->>FrioProc: remove frames
  Worker->>PGP: status sucesso + ref. zip
  Worker->>Bus: publica tópico processamento-video-atualizado (sucesso)
  Bus->>Up: consome tópico processamento-video-atualizado (sucesso); remove original
```
