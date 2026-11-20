# ADRs do sistema

Decisões transversais aos microsserviços. ADRs de implementação local ficam no repo dono. Limites de mídia e retenção: [RF21–RF24](../especificacao/02_requisitos-funcionais.md).

| ADR | Tema | Status |
|-----|------|--------|
| [001](001-storage-multi-tier.md) | Storage multi-tier / posse de buckets (Standard na demo) | Aceito |
| [002](002-progresso-cache-bff.md) | Progresso canônico + Redis só no `bff` (cache-aside) | Aceito |
| [003](003-outbox-inbox.md) | Outbox + inbox idempotente | Aceito |
| [004](004-processamento-checkpoint-retomada.md) | Split `processamento-api`/`processamento-worker` + checkpoint/retomada | Aceito |
| [005](005-sendgrid.md) | Provider de e-mail SendGrid | Aceito |

A numeração dos **ADRs** é sequencial de decisão; pastas temáticas do repo **não** usam prefixo numérico.
