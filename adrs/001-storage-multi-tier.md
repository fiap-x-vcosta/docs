# ADR 001: Storage multi-tier e posse de buckets

**Data:** 20 de novembro de 2026  
**Status:** Aceito  
**Autores:** Victor Costa

## 1. Contexto e Problema

O ciclo trata vídeo original, frames, zip e thumbs com perfis de acesso distintos ([RNF03](../especificacao/03_requisitos-nao-funcionais.md), [RNF07](../especificacao/03_requisitos-nao-funcionais.md)). Misturar tudo num único bucket público ou compartilhar IAM de escrita entre serviços acopla posse e custo.

O problema a ser resolvido é: **Como organizar object storage frio/quente, posse por serviço e provisionamento na demo GCP?**

## 2. Decisão

- **Três buckets de mídia** (nomes finais no Terraform):
  - **Frio `upload`:** vídeo original (classe **Nearline**).
  - **Quente `upload`:** thumbs de catálogo (classe **Standard**).
  - **Frio `processamento`:** frames e zip (classe **Nearline**).
- **Front estático** (`web`): bucket/hosting **separado**, sem mídia de domínio.
- **Posse:** só o serviço dono grava/apaga no próprio bucket ([RNF07](../especificacao/03_requisitos-nao-funcionais.md)). `processamento-worker` lê o original só via **URL assinada** emitida por `upload` ([RNF16](../especificacao/03_requisitos-nao-funcionais.md)). Thumb chega a `upload` pelo tópico `processamento-video-atualizado` (`iniciado` + `frameReadUrl`), não por escrita cross-bucket.
- **Sem ACL pública** nos buckets de mídia. Download do zip e leitura do original: **URLs assinadas** ([RNF15](../especificacao/03_requisitos-nao-funcionais.md)).
- **Lifecycle:** zip com retenção de **7 dias** após sucesso ([RF23](../especificacao/02_requisitos-funcionais.md)); original e frames removidos ao concluir o ciclo ([RF19](../especificacao/02_requisitos-funcionais.md)).
- **Chaves determinísticas** no frio de `processamento` (ex. `{videoId}/frames/{indice}.jpg`, `{videoId}/frames.zip`) para retomada idempotente ([ADR 004](004-processamento-checkpoint-retomada.md)).
- **Provisionamento (modelo C):** buckets de mídia sobem/descem no ciclo **demo-up / demo-down** (recurso tipo `infra-storage` / fase do `e2e`), **não** no bootstrap “eterno”.

## 3. Justificativa

- Nearline no frio reduz custo de objetos grandes; Standard no quente serve listagem com latência aceitável.
- Posse por bucket evita IAM cruzado e deixa limpeza alinhada a `upload` / `processamento-worker`.
- Demo-up/down evita custo permanente de buckets fora da janela de apresentação.

## 4. Alternativas Consideradas

- **Um bucket único com prefixos:** simples; mistura posse, IAM e lifecycle.
- **Buckets no `infra-bootstrap` permanente:** estável; custo ocioso fora da demo.
- **Coldline/Archive no frio:** mais barato; latência e restore inadequados para o worker ler o original na demo.
- **Thumbs escritas por `processamento-worker` no quente de `upload`:** atalho; viola posse cross-domínio.

## 5. Consequências

### Positivas

- Custo e lifecycle separados por perfil de acesso.
- Contratos de leitura entre serviços só via URL assinada ou evento.

### Negativas / Riscos (Mitigações)

- **Mais buckets e IAM para provisionar na demo.**
  - *Mitigação:* um módulo `infra-storage` no demo-up/down; outputs consumidos pelos TF dos serviços.
- **Nearline com custo de early delete se o ciclo for muito curto.**
  - *Mitigação:* aceitável no MVP acadêmico; lifecycle e RF19 limitam retenção.
