# FIAP X — Documentação do sistema

Documentação **transversal** da org [`fiap-x-vcosta`](https://github.com/fiap-x-vcosta): requisitos, linguagem de domínio, event storming, fluxos, C4, catálogo de eventos e ADRs.

Docs específicas de cada microsserviço (incluindo modelo de dados) ficam no repositório do serviço. Contratos HTTP públicos ficam no **Swagger** de cada API (este repo não duplica OpenAPI).

## Índice

| Pasta / doc | Conteúdo | Status |
|-------------|----------|--------|
| [especificacao/01_linguagem-onipresente.md](especificacao/01_linguagem-onipresente.md) | Glossário do domínio | Preenchido |
| [especificacao/02_requisitos-funcionais.md](especificacao/02_requisitos-funcionais.md) | Requisitos funcionais | Preenchido |
| [especificacao/03_requisitos-nao-funcionais.md](especificacao/03_requisitos-nao-funcionais.md) | Requisitos não funcionais | Preenchido |
| [event-storming/](event-storming/) | Event storming (board, mermaid, artefatos) | Preenchido |
| [fluxo-e-estados/](fluxo-e-estados/) | Sequências Mermaid (feliz, erros, cancelamento) | Preenchido |
| [arquitetura/](arquitetura/) | C4 PlantUML + SVG (contexto, containers, componentes) | Preenchido |
| [eventos/catalogo-de-eventos.md](eventos/catalogo-de-eventos.md) | Tópicos, payloads, idempotência | Preenchido |
| [adrs/](adrs/) | Decisões arquiteturais de sistema (001–005) | Preenchido |

## Convenções

- Na raiz do repo: apenas `README.md`, `AGENTS.md` e `.gitignore` (mais `.cursor/` para regras de agente).
- Conteúdo agrupado por pasta temática; nomes por extenso (sem abreviações do tipo `rf-rnf`).
- Serviços em forma canônica minúscula: `autenticacao`, `upload`, `processamento-api`, `processamento-worker`, `notificacao`, `bff`.
- Alterações via **branch → PR**; o **humano** revisa e faz o **merge** em `main` (a IA não mergeia).
- Naming e demais regras: ver [`AGENTS.md`](AGENTS.md) e [`.cursor/rules/`](.cursor/rules/).
