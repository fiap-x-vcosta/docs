# FIAP X — Documentação do sistema

Documentação **transversal** da org [`fiap-x-vcosta`](https://github.com/fiap-x-vcosta): requisitos, linguagem de domínio, event storming, fluxos, C4, catálogo de eventos, modelo de dados, limites de mídia e ADRs.

Docs específicas de cada microsserviço ficam no repositório do serviço. Contratos HTTP públicos ficam no **Swagger** de cada API (este repo não duplica OpenAPI).

## Índice

| Doc | Conteúdo | Status |
|-----|----------|--------|
| [00_linguagem-onipresente.md](00_linguagem-onipresente.md) | Glossário do domínio | Stub — Parte 1/2 |
| [01_requisitos-funcionais-e-nao-funcionais.md](01_requisitos-funcionais-e-nao-funcionais.md) | RF e RNF | Stub — Parte 2 |
| [02_event-storming/](02_event-storming/) | Event storming (board, mermaid, artefatos) | Stub — Parte 1 |
| [03_fluxo-e-estados/](03_fluxo-e-estados/) | Happy path, erro → e-mail, máquina de estados | Stub — Parte 3 |
| [04_arquitetura/](04_arquitetura/) | C4 (contexto, containers, componentes) | Stub — Parte 4 |
| [05_catalogo-de-eventos.md](05_catalogo-de-eventos.md) | Tópicos, payloads, idempotência | Stub — Parte 5 |
| [06_modelo-de-dados.md](06_modelo-de-dados.md) | ER / posse por serviço | Stub — Parte 6 |
| [07_limites-de-midia.md](07_limites-de-midia.md) | Limites e amostragem (fonte de verdade na GCP) | Stub — Parte 7 |
| [adrs/](adrs/) | Decisões arquiteturais de sistema | Stub — Partes 5–7 |

## Convenções

- Prefixo numérico na ordem de leitura (padrão da org de referência).
- Nomes por extenso (sem abreviações do tip `rf-rnf`).
- Alterações via **branch → PR → merge** em `main` (sem commit direto).
- Naming: ver regras em `AGENTS.md` e no workspace local `contexto.md`.
