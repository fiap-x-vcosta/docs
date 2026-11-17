# Guia para agentes — `docs` (FIAP X)

Documentação transversal do sistema de processamento de vídeos **FIAP X**. Org [`fiap-x-vcosta`](https://github.com/fiap-x-vcosta).

## Antes de mudar docs

1. Não inventar requisito fora do enunciado + decisões fechadas no workspace local (`contexto.md` / `checklist.md`).
2. Se P1–P6 ou desenho de domínio estiver ambíguo, **perguntar** antes de endurecer contrato.
3. **Git:** nunca commit/push direto em `main` — branch → PR → merge.
4. Docs de um microsserviço (Swagger local, runbook do serviço) **não** moram aqui — só o que é transversal.
5. Não criar `contratos-http` neste repo (Swagger por serviço).
6. Proibição de naming: não usar a string proibida que combina o produto com “videos” em camel/kebab/snake em artefato algum. Preferir `fiap-x`, `processamento`, `midia`, `frames`, etc.

## Layout

| Peça | Papel |
|------|--------|
| `README.md` | Índice |
| `00_` … `07_` | Artefatos de produto / arquitetura na ordem de leitura |
| `02_event-storming/` | Board e derivados do storm |
| `03_fluxo-e-estados/` | Fluxos e estados |
| `04_arquitetura/` | C4 |
| `adrs/` | ADRs de sistema (storage, progresso/Redis, outbox, worker, SendGrid, …) |

## Fora de escopo

Código de microsserviços, Terraform GCP, pipelines e compose e2e (só referenciados pelos diagramas quando existir).
