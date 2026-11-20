# Requisitos não funcionais

Glossário: [linguagem onipresente](01_linguagem-onipresente.md).

## 19 de novembro de 2026

### Arquitetura e persistência

- **RNF01** — Arquitetura em **microsserviços** (`autenticacao`, `upload`, `processamento-api`, `processamento-worker`, `notificacao` e `bff`).
- **RNF02** — Dados de domínio **persistidos** (PostgreSQL por serviço dono, conforme ADRs).
- **RNF03** — Mídia em **object storage multi-tier** (frio para vídeo/frames/zip; quente para thumbs de catálogo — detalhe em ADR de storage).
- **RNF04** — Única **API HTTP pública** de produto é o `bff`; demais serviços apenas rede interna. Front estático é superfície pública separada (HTML/JS).
- **RNF05** — Autenticação com JWT **RS256** e **JWKS**; chave privada só em `autenticacao`.
- **RNF06** — Superfícies públicas (front e `bff`) expostas apenas via **HTTPS**.
- **RNF07** — **Posse de object storage por domínio:** `upload` possui frio do **vídeo original** e quente das **thumbs** de catálogo; `processamento-worker` possui frio de **frames/zip**. Sem escrita ou exclusão cross-domínio (thumbs chegam a `upload` via tópico `processamento-video-atualizado` / `iniciado`).

### Mensageria e resiliência

- **RNF08** — Integração assíncrona via **Google Pub/Sub** (um tópico por evento de domínio, conforme catálogo).
- **RNF09** — Publicação de eventos de integração com **transactional outbox**; consumidores **idempotentes**.
- **RNF10** — Subscriptions Pub/Sub com **DLQ** após esgotar retries.
- **RNF11** — Processamento separado em `processamento-api` (consultas HTTP de status/progresso) e `processamento-worker` (FFmpeg, gravação de mídia, checkpoint).
- **RNF12** — `processamento-worker` com **checkpoint/retomada** (estado de controle + objetos de mídia duráveis), para não perder trabalho em falha de pod.
- **RNF13** — Extração de frames com **FFmpeg** (ou equivalente) em `processamento-worker`.

### Progresso, download, leitura de mídia e cache

- **RNF14** — Progresso do processamento é **canônico** em `processamento-api` (PostgreSQL). A **projeção de status** para listagem/paginação fica em `upload` (atualizada pelo tópico `processamento-video-atualizado`; em cancelamento, pelo próprio `upload` ao chamar `processamento-api`). O `bff` **não** compartilha esses bancos e pode manter **cache-aside** próprio (ex. Redis) só para leituras de progresso.
- **RNF15** — Download do zip via **URL assinada**, **sem** bucket público; enquanto o zip existir (retenção em [RF23](02_requisitos-funcionais.md)), o dono pode obter URL utilizável. Autorização: [RF17](02_requisitos-funcionais.md).
- **RNF16** — `processamento-worker` obtém o **vídeo original** apenas via **URL assinada de leitura** emitida por `upload` (tópico `upload-video-atualizado` ou API); sem IAM de leitura direta no bucket de `upload`.
- **RNF17** — A **remoção do vídeo original** é responsabilidade de `upload`, ao consumir o tópico `processamento-video-atualizado` de conclusão do ciclo (`sucesso`, `falha` ou `cancelado`).

### Qualidade, entrega e operação

- **RNF18** — Código versionado no **GitHub** (org do projeto); mudanças via PR.
- **RNF19** — Testes automatizados; cobertura unitária ≥ **80%** line+branch por assembly (.NET).
- **RNF20** — **CI** automático (lint, testes, Sonar); provisionamento/deploy caros na GCP são **manuais** (janela de demo).
- **RNF21** — Observabilidade com **Datadog** (traces atravessando HTTP e Pub/Sub).
- **RNF22** — E-mail via **SendGrid**.
