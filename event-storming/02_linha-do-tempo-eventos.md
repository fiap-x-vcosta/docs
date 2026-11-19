# Linha do tempo de eventos

Transcrição de [`02_linha-do-tempo-eventos.jpg`](02_linha-do-tempo-eventos.jpg).

Caminho feliz: setas pretas. Exceções: setas vermelhas tracejadas.

## Mermaid

```mermaid
flowchart LR
  classDef ev fill:#F9E79F,stroke:#333,color:#000

  Criado["Usuário Criado"]
  Logado["Usuário Logado"]
  Upload["Upload de Vídeo Feito"]
  TriagemOk["Triagem de Vídeo Concluída"]
  Rejeitado["Vídeo Rejeitado na Triagem"]
  Persistido["Vídeo Persistido"]
  FalhaPersistir["Falha ao Persistir"]
  Enfileirado["Vídeo Enfileirado para Processamento"]
  Iniciado["Processamento do Vídeo Iniciado"]
  Cancelado["Processamento do Vídeo Cancelado"]
  FalhaProc["Vídeo Processado com Falha"]
  Sucesso["Vídeo Processado com Sucesso"]
  Zip["Imagens Salvas em .zip"]
  TempDel["Imagens Temporárias Deletadas"]
  Url["URL de Download Gerada"]
  NotifOk["Notificação de Sucesso Enviada"]
  NotifFalha["Notificação de Falha Enviada"]

  Criado --> Logado
  Logado --> Upload
  Upload --> TriagemOk
  TriagemOk --> Persistido
  Persistido --> Enfileirado
  Enfileirado --> Iniciado
  Iniciado --> Sucesso
  Sucesso --> Zip
  Zip --> TempDel
  Zip --> Url
  Url --> NotifOk

  Upload -.-> Rejeitado
  Rejeitado -.-> NotifFalha
  TriagemOk -.-> FalhaPersistir
  FalhaPersistir -.-> NotifFalha
  Iniciado -.-> Cancelado
  Iniciado -.-> FalhaProc
  Cancelado -.-> TempDel
  FalhaProc -.-> TempDel
  FalhaProc -.-> NotifFalha

  class Criado,Logado,Upload,TriagemOk,Rejeitado,Persistido,FalhaPersistir,Enfileirado,Iniciado,Cancelado,FalhaProc,Sucesso,Zip,TempDel,Url,NotifOk,NotifFalha ev

  linkStyle 0,1,2,3,4,5,6,7,8,9,10 stroke:#000,stroke-width:2px
  linkStyle 11,12,13,14,15,16,17,18,19 stroke:#C0392B,stroke-width:2px,stroke-dasharray:6 4
```

## Arestas

### Feliz
- Usuário Criado → Usuário Logado
- Usuário Logado → Upload de Vídeo Feito
- Upload de Vídeo Feito → Triagem de Vídeo Concluída
- Triagem de Vídeo Concluída → Vídeo Persistido
- Vídeo Persistido → Vídeo Enfileirado para Processamento
- Vídeo Enfileirado para Processamento → Processamento do Vídeo Iniciado
- Processamento do Vídeo Iniciado → Vídeo Processado com Sucesso
- Vídeo Processado com Sucesso → Imagens Salvas em .zip
- Imagens Salvas em .zip → Imagens Temporárias Deletadas *(em paralelo com URL)*
- Imagens Salvas em .zip → URL de Download Gerada *(em paralelo com delete temp)*
- URL de Download Gerada → Notificação de Sucesso Enviada

### Exceção
- Upload de Vídeo Feito → Vídeo Rejeitado na Triagem → Notificação de Falha Enviada
- Triagem de Vídeo Concluída → Falha ao Persistir → Notificação de Falha Enviada
- Processamento do Vídeo Iniciado → Processamento do Vídeo Cancelado → Imagens Temporárias Deletadas  
  *(cancelamento **não** gera notificação)*
- Processamento do Vídeo Iniciado → Vídeo Processado com Falha → Imagens Temporárias Deletadas
- Vídeo Processado com Falha → Notificação de Falha Enviada
