# Guia para agentes — `docs` (FIAP X)

Documentação transversal do sistema de processamento de vídeos **FIAP X**. Org [`fiap-x-vcosta`](https://github.com/fiap-x-vcosta).

## Antes de mudar docs

1. Não inventar requisito fora do enunciado + decisões fechadas no workspace local (`contexto.md` / `checklist.md`).
2. Se P1–P6 ou desenho de domínio estiver ambíguo, **perguntar** antes de endurecer contrato.
3. **Git:** nunca commit/push direto em `main` — branch → PR → merge (ver [`.cursor/rules/git-workflow.mdc`](.cursor/rules/git-workflow.mdc)).
4. Docs de um microsserviço (Swagger local, runbook do serviço) **não** moram aqui — só o que é transversal.
5. Não criar catálogo OpenAPI / `contratos-http` neste repo (Swagger por serviço).
6. Proibição de naming: não usar a string proibida que combina o produto com “videos” em camel/kebab/snake em artefato algum. Preferir `fiap-x`, `processamento`, `midia`, `frames`, etc.

## Layout

| Peça | Papel |
|------|--------|
| `README.md` | Índice |
| `dominio/` | Linguagem onipresente e modelo de dados |
| `requisitos/` | RF/RNF e limites de mídia |
| `event-storming/` | Board e derivados do storm |
| `fluxo-e-estados/` | Fluxos e estados |
| `arquitetura/` | C4 |
| `eventos/` | Catálogo de eventos |
| `adrs/` | ADRs de sistema (storage, progresso/Redis, outbox, worker, SendGrid, …) |
| `.cursor/rules/` | Regras obrigatórias para agentes |

## Fora de escopo

Código de microsserviços, Terraform GCP, pipelines e compose e2e (só referenciados pelos diagramas quando existir).
