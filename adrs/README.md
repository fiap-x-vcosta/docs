# ADRs do sistema

Decisões transversais aos microsserviços. ADRs de implementação local ficam no repo dono.

| ADR | Tema | Status |
|-----|------|--------|
| 001 | Storage multi-tier / `infra-storage` no demo-up/down | Planejado |
| 002 | Progresso de frames + Redis só no BFF (cache-aside) | Planejado |
| 003 | Outbox + inbox idempotente | Planejado |
| 004 | Worker API split + checkpoint/retomada (Postgres + frio) | Planejado |
| 005 | Provider de e-mail SendGrid | Planejado |

Arquivos `001-….md` … são criados quando a decisão for escrita. A numeração dos **ADRs** é sequencial de decisão; pastas temáticas do repo **não** usam prefixo numérico.
