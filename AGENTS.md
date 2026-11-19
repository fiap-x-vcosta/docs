# Guia para agentes — `docs` (FIAP X)

Documentação transversal do sistema de processamento de vídeos **FIAP X**. Org [`fiap-x-vcosta`](https://github.com/fiap-x-vcosta).

## Antes de mudar docs

1. Não inventar requisito fora do enunciado + decisões já publicadas **neste repo** (ou em outros repos da org).
2. Se domínio, eventos ou ADRs estiverem ambíguos, **perguntar** antes de endurecer contrato.
3. **Git:** nunca commit/push direto em `main`; abrir PR e **aguardar o humano mergear** (ver [`.cursor/rules/git-workflow.mdc`](.cursor/rules/git-workflow.mdc)).
4. Texto **publicado** autocontido na org — a IA pode usar notas fora da org como contexto, mas não deve mencioná-las nos arquivos do repo (ver [`.cursor/rules/org-only-canon.mdc`](.cursor/rules/org-only-canon.mdc)).
5. Docs de um microsserviço (Swagger local, runbook do serviço) **não** moram aqui — só o que é transversal.
6. Não criar catálogo OpenAPI / `contratos-http` neste repo (Swagger por serviço).
7. Proibição de naming: não usar a string proibida que combina o produto com “videos” em camel/kebab/snake em artefato algum. Preferir `fiap-x`, `processamento`, `midia`, `frames`, etc.

## Layout

| Peça | Papel |
|------|--------|
| `README.md` | Índice |
| `especificacao/` | Linguagem onipresente, RF e RNF (`01_`…`03_`; limites de mídia na seção de RF) |
| `dominio/` | Modelo de dados |
| `event-storming/` | Board Miro (jpg) + transcrição Markdown canônica (ver regra `event-storming-artifacts`) |
| `fluxo-e-estados/` | Fluxos e estados |
| `arquitetura/` | C4 |
| `eventos/` | Catálogo de eventos |
| `adrs/` | ADRs de sistema (storage, progresso/Redis, outbox, worker, SendGrid, …) |
| `.cursor/rules/` | Regras obrigatórias para agentes |

## Fora de escopo

Código de microsserviços, Terraform GCP, pipelines e compose e2e (só referenciados pelos diagramas quando existir).
