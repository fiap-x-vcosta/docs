# FIAP X — Documentação do sistema

Documentação **transversal** da org [`fiap-x-vcosta`](https://github.com/fiap-x-vcosta): requisitos, linguagem de domínio, event storming, fluxos, C4, catálogo de eventos, modelo de dados, limites de mídia e ADRs.

Docs específicas de cada microsserviço ficam no repositório do serviço. Contratos HTTP públicos ficam no **Swagger** de cada API (este repo não duplica OpenAPI).

## Índice

| Pasta / doc | Conteúdo | Status |
|-------------|----------|--------|
| [dominio/linguagem-onipresente.md](dominio/linguagem-onipresente.md) | Glossário do domínio | Stub — Parte 1/2 |
| [dominio/modelo-de-dados.md](dominio/modelo-de-dados.md) | ER / posse por serviço | Stub — Parte 6 |
| [requisitos/requisitos-funcionais-e-nao-funcionais.md](requisitos/requisitos-funcionais-e-nao-funcionais.md) | RF e RNF | Stub — Parte 2 |
| [requisitos/limites-de-midia.md](requisitos/limites-de-midia.md) | Limites e amostragem (fonte de verdade na GCP) | Stub — Parte 7 |
| [event-storming/](event-storming/) | Event storming (board, mermaid, artefatos) | Stub — Parte 1 |
| [fluxo-e-estados/](fluxo-e-estados/) | Happy path, erro → e-mail, máquina de estados | Stub — Parte 3 |
| [arquitetura/](arquitetura/) | C4 (contexto, containers, componentes) | Stub — Parte 4 |
| [eventos/catalogo-de-eventos.md](eventos/catalogo-de-eventos.md) | Tópicos, payloads, idempotência | Stub — Parte 5 |
| [adrs/](adrs/) | Decisões arquiteturais de sistema | Stub — Partes 5–7 |

## Convenções

- Na raiz do repo: apenas `README.md`, `AGENTS.md` e `.gitignore` (mais `.cursor/` para regras de agente).
- Conteúdo agrupado por pasta temática; nomes por extenso (sem abreviações do tipo `rf-rnf`).
- Alterações via **branch → PR → merge** em `main` (sem commit direto).
- Naming: ver `AGENTS.md` e o workspace local `contexto.md`.
