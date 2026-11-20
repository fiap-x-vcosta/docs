# ADR 005: Provider de e-mail SendGrid

**Data:** 20 de novembro de 2026  
**Status:** Aceito  
**Autores:** Victor Costa

## 1. Contexto e Problema

O MVP envia e-mail de **sucesso** e de **falha** (com motivo) ([RF05](../especificacao/02_requisitos-funcionais.md), [RF07](../especificacao/02_requisitos-funcionais.md), [RF12](../especificacao/02_requisitos-funcionais.md), [RF16](../especificacao/02_requisitos-funcionais.md), [RNF22](../especificacao/03_requisitos-nao-funcionais.md)). Cancelamento **não** notifica. É preciso um provider gerenciável na demo sem SMTP self-hosted.

O problema a ser resolvido é: **Qual provider de e-mail usar e como `notificacao` o integra?**

## 2. Decisão

- Provider: **SendGrid** (API HTTP).
- Serviço dono: `notificacao` consome `upload-video-atualizado` (`resultado=falha`) e `processamento-video-atualizado` (`status=sucesso` \| `falha`).
- Destinatário: `notificacao` resolve o e-mail via `autenticacao`/`userId` (payload **sem** endereço).
- Credencial: **API key** em Secret Manager / secret de runtime; nunca no Git.
- Ambiente de desenvolvimento: sandbox/conta free SendGrid; remetente verificado conforme regra da conta.
- Envio **idempotente** por `eventId` (inbox + não reenviar o mesmo evento) ([ADR 003](003-outbox-inbox.md)).
- Detalhe de templates HTML e domínio de remetente de produção: implementação em `notificacao-service` (§12).

## 3. Justificativa

- SendGrid é o padrão já previsto nos RNFs e no desenho C4.
- Separar resolução de e-mail em `autenticacao` evita PII no barramento.
- Idempotência evita spam em redelivery Pub/Sub.

## 4. Alternativas Consideradas

- **SMTP genérico / Mailhog só local:** bom para teste; não cobre a demo GCP sem outro provedor.
- **Amazon SES / outro SaaS:** equivalente; fora do stack GCP já adotado e da escolha travada.
- **E-mail embutido em `upload`/`processamento`:** acopla falha de SMTP ao domínio de mídia.

## 5. Consequências

### Positivas

- Um único lugar (`notificacao`) fala com o provider.
- Contratos de evento já filtram sucesso/falha sem tópico dedicado de “notificação solicitada”.

### Negativas / Riscos (Mitigações)

- **Conta/remetente SendGrid a criar antes da §12 completa.**
  - *Mitigação:* na §1 só este ADR; provisionar chave na implementação de `notificacao`.
- **Limites da free tier.**
  - *Mitigação:* volume da demo é baixo; monitorar rejeições.
