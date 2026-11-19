# Requisitos não funcionais

Glossário: [linguagem onipresente](01_linguagem-onipresente.md).

## 19 de novembro de 2026

### Arquitetura e persistência

- **RNF01** — Arquitetura em **microsserviços** (autenticação, triagem/upload, processamento, notificação e BFF de consultas).
- **RNF02** — Dados de domínio **persistidos** (PostgreSQL por serviço dono, conforme ADRs).
- **RNF03** — Mídia em **object storage multi-tier** (armazenamento frio para vídeo/frames/zip; quente para thumbs — detalhe em ADR de storage).
- **RNF04** — Única **API HTTP pública** de produto é o **BFF**; demais serviços apenas rede interna. Front estático é superfície pública separada (HTML/JS).
- **RNF05** — Autenticação com JWT **RS256** e **JWKS**; chave privada só no serviço de autenticação.
- **RNF06** — Superfícies públicas (front e BFF) expostas apenas via **HTTPS**.

### Mensageria e resiliência

- **RNF07** — Integração assíncrona via **Google Pub/Sub** (um tópico por evento de domínio, conforme catálogo).
- **RNF08** — Publicação de eventos de integração com **transactional outbox**; consumidores **idempotentes**.
- **RNF09** — Subscriptions Pub/Sub com **DLQ** após esgotar retries.
- **RNF10** — Worker de processamento com **checkpoint/retomada** (estado de controle + objetos de mídia duráveis), para não perder trabalho em falha de pod.
- **RNF11** — Extração de frames com **FFmpeg** (ou equivalente) no worker de processamento.

### Progresso, download e cache

- **RNF12** — Progresso e estado de processamento **canônicos** no serviço dono (PostgreSQL); o BFF **não** compartilha esse banco e pode manter **cache-aside** próprio (ex. Redis) só para leituras.
- **RNF13** — Download do zip via **URL assinada**, **sem** bucket público; enquanto o zip existir (retenção em [RF23](02_requisitos-funcionais.md)), o dono pode obter URL utilizável. Autorização: [RF17](02_requisitos-funcionais.md).


### Qualidade, entrega e operação

- **RNF14** — Código versionado no **GitHub** (org do projeto); mudanças via PR.
- **RNF15** — Testes automatizados; cobertura unitária ≥ **80%** line+branch por assembly (.NET).
- **RNF16** — **CI** automático (lint, testes, Sonar); provisionamento/deploy caros na GCP são **manuais** (janela de demo).
- **RNF17** — Observabilidade com **Datadog** (traces atravessando HTTP e Pub/Sub).
- **RNF18** — E-mail via **SendGrid**.
