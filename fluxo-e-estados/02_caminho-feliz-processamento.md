```mermaid
sequenceDiagram
  actor U as Usuário
  participant Web as Front
  participant BFF as BFF
  participant Redis as Redis BFF
  participant Bus as Pub/Sub
  participant Up as Upload
  participant Quente as Storage quente Upload
  participant Api as Processamento API
  participant Worker as Processamento Worker
  participant PGP as Postgres Processamento
  participant FrioUp as Storage frio Upload
  participant FrioProc as Storage frio Processamento

  Note over Bus,Worker: persistencia-video-finalizada (sucesso)
  Bus->>Worker: consome (filtro sucesso)
  Worker->>FrioUp: GET original via URL assinada
  Worker->>PGP: status processando + total estimado + checkpoint
  Worker->>Bus: publica processamento-video-status (em-processamento + frameReadUrl)
  Bus->>Up: status processando
  Up->>Quente: grava thumb

  loop a cada 30 frames da fonte
    Worker->>Worker: FFmpeg extrai frame
    Worker->>FrioProc: grava frame
    Worker->>PGP: progresso N/total + checkpoint
  end

  U->>Web: consulta listagem
  Web->>BFF: GET lista (JWT, página)
  BFF->>Up: página de vídeos (projeção de status + thumb)
  alt item processando
    BFF->>Redis: busca progresso em cache
    alt cache miss
      BFF->>Api: progresso N/total
      Api->>PGP: lê progresso canônico
      Api-->>BFF: N/total
      BFF->>Redis: grava cache-aside
    end
  end
  BFF-->>Web: itens + status + progresso opcional + thumb
  Web-->>U: lista

  Worker->>FrioProc: grava zip
  Worker->>FrioProc: remove frames
  Worker->>PGP: status sucesso + ref. zip
  Worker->>Bus: publica processamento-video-status (sucesso)
  Bus->>Up: status sucesso; remove original
```
