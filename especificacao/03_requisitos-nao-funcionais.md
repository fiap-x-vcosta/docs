# Requisitos não funcionais

Glossário: [linguagem onipresente](01_linguagem-onipresente.md).

## 19 de novembro de 2026

### Arquitetura e persistência

- **RNF01** — Arquitetura em **microsserviços** (autenticação, triagem/upload, processamento, notificação e BFF de consultas).
- **RNF02** — Dados de domínio **persistidos** (PostgreSQL por serviço dono, conforme ADRs).
- **RNF03** — Mídia em **object storage multi-tier** (armazenamento frio para vídeo/frames/zip; quente para thumbs — detalhe em ADR de storage).
- **RNF04** — Única **API HTTP pública** de produto é o **BFF**; demais serviços apenas rede interna. Front estático é superfície pública separada (HTML/JS).
- **RNF05** — Autenticação com JWT **RS256** e **JWKS**; chave privada só no serviço de autenticação.

### Mensageria e resiliência

- **RNF06** — Integração assíncrona via **Google Pub/Sub** (um tópico por evento de domínio, conforme catálogo).
- **RNF07** — Publicação de eventos de integração com **transactional outbox**; consumidores **idempotentes**.
- **RNF08** — Worker de processamento com **checkpoint/retomada** (estado de controle + objetos de mídia duráveis), para não perder trabalho em falha de pod.

### Qualidade, entrega e operação

- **RNF09** — Código versionado no **GitHub** (org do projeto); mudanças via PR.
- **RNF10** — Testes automatizados; cobertura unitária ≥ **80%** line+branch por assembly (.NET).
- **RNF11** — **CI** automático (lint, testes, Sonar); provisionamento/deploy caros na GCP são **manuais** (janela de demo).
- **RNF12** — Observabilidade com **Datadog** (traces atravessando HTTP e Pub/Sub).
- **RNF13** — E-mail via **SendGrid**.
- **RNF14** — O BFF não compartilha banco com outros serviços; pode usar cache próprio para leituras (ex. progresso).
