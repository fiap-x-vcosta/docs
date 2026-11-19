# FIAP X — Documentação do sistema

Documentação **transversal** da org [`fiap-x-vcosta`](https://github.com/fiap-x-vcosta): requisitos, linguagem de domínio, event storming, fluxos, C4, catálogo de eventos, modelo de dados, limites de mídia e ADRs.

Docs específicas de cada microsserviço ficam no repositório do serviço. Contratos HTTP públicos ficam no **Swagger** de cada API (este repo não duplica OpenAPI).

## Índice

| Pasta / doc | Conteúdo | Status |
|-------------|----------|--------|
| [especificacao/01_linguagem-onipresente.md](especificacao/01_linguagem-onipresente.md) | Glossário do domínio | Preenchido |
| [especificacao/02_requisitos-funcionais.md](especificacao/02_requisitos-funcionais.md) | Requisitos funcionais | Preenchido |
| [especificacao/03_requisitos-nao-funcionais.md](especificacao/03_requisitos-nao-funcionais.md) | Requisitos não funcionais | Preenchido |
| [dominio/modelo-de-dados.md](dominio/modelo-de-dados.md) | ER / posse por serviço | Stub |
| [requisitos/limites-de-midia.md](requisitos/limites-de-midia.md) | Limites e amostragem (fonte de verdade na GCP) | Stub |
| [event-storming/](event-storming/) | Event storming (board, mermaid, artefatos) | Preenchido |
| [fluxo-e-estados/](fluxo-e-estados/) | Happy path, erro → e-mail, máquina de estados | Stub |
| [arquitetura/](arquitetura/) | C4 (contexto, containers, componentes) | Stub |
| [eventos/catalogo-de-eventos.md](eventos/catalogo-de-eventos.md) | Tópicos, payloads, idempotência | Stub |
| [adrs/](adrs/) | Decisões arquiteturais de sistema | Stub |

## Convenções

- Na raiz do repo: apenas `README.md`, `AGENTS.md` e `.gitignore` (mais `.cursor/` para regras de agente).
- Conteúdo agrupado por pasta temática; nomes por extenso (sem abreviações do tipo `rf-rnf`).
- Alterações via **branch → PR**; o **humano** revisa e faz o **merge** em `main` (a IA não mergeia).
- Naming e demais regras: ver [`AGENTS.md`](AGENTS.md) e [`.cursor/rules/`](.cursor/rules/).
