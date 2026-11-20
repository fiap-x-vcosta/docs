# ADR 003 — Outbox transacional e inbox idempotente

## Status

Aceito

## Contexto

Serviços publicam fatos de domínio no Pub/Sub e outros reagem (processar, limpar mídia, enviar e-mail). Gravar estado e publicar na mesma requisição sem coordenação gera perda ou duplicata de mensagem. Consumidores pull/streaming podem receber a mesma mensagem mais de uma vez ([RNF08](../especificacao/03_requisitos-nao-funcionais.md)–[RNF10](../especificacao/03_requisitos-nao-funcionais.md)).

## Decisão

1. **Transactional outbox** no PostgreSQL do serviço publicador: na mesma transação do estado de domínio, inserir a linha de outbox com o envelope do [catálogo](../eventos/catalogo-de-eventos.md) (`eventId`, `type`, `version`, `occurredAt`, `data`).
2. **Relay** (processo/hosted service do próprio serviço) lê outbox pendente, publica no tópico Pub/Sub cujo nome é o `type`, e marca a linha como publicada (ou remove). Falha de publish mantém a linha para retry.
3. **Inbox** no PostgreSQL de cada consumidor: antes de aplicar o efeito, tentar gravar `eventId` (unique). Se já existir, ack sem reaplicar. Efeito de negócio + inbox na mesma transação quando o efeito for transacional; para I/O externo (storage, SendGrid), garantir ação idempotente e registrar inbox após sucesso.
4. **Subscriptions** com retry e **DLQ** após esgotar tentativas ([RNF10](../especificacao/03_requisitos-nao-funcionais.md)).
5. Publicadores e consumidores **não** compartilham tabelas de outbox/inbox entre serviços.

## Consequências

- Consistência: estado canônico e intenção de publish sobem juntos; o relay pode atrasar a entrega sob carga.
- Duplicatas na entrega são esperadas; o contrato de correção é idempotência por `eventId`.
- Cada serviço dono provisiona schema de outbox e/ou inbox no seu Postgres.
- Ordem entre tópicos distintos não é garantida; consumidores não devem assumir ordem global.
