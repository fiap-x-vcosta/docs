# ADRs do sistema

Decisões transversais aos microsserviços. ADRs de implementação local ficam no repo dono.

| ADR | Tema | Status |
|-----|------|--------|
| 001 | Storage multi-tier / `infra-storage` no demo-up/down | Planejado — Parte 7 |
| 002 | Progresso de frames + Redis só no BFF (cache-aside) | Planejado — Parte 7 |
| 003 | Outbox + inbox idempotente | Planejado — Parte 5/7 |
| 004 | Worker API split + checkpoint/retomada (Postgres + frio) | Planejado — Parte 7 |
| 005 | Provider de e-mail SendGrid | Planejado — Parte 7 |

Arquivos `001-….md` … entram nas partes correspondentes (não inventar decisão aqui no skeleton). A numeração dos **ADRs** é sequencial de decisão; pastas temáticas do repo **não** usam prefixo numérico.
