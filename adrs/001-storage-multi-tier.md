# ADR 001: Storage multi-tier e posse de buckets

**Data:** 20 de novembro de 2026  
**Status:** Aceito  
**Autores:** Victor Costa

## 1. Contexto e Problema

O ciclo trata vídeo original, frames, zip e thumbs com perfis de acesso distintos ([RNF03](../especificacao/03_requisitos-nao-funcionais.md), [RNF07](../especificacao/03_requisitos-nao-funcionais.md)). Misturar tudo num único bucket público ou compartilhar IAM de escrita entre serviços acopla posse e custo. Classes com retenção mínima (Nearline/Coldline) geram early delete na demo-up/down no mesmo dia.

O problema a ser resolvido é: **Como organizar object storage frio/quente, posse por serviço e provisionamento na demo GCP?**

## 2. Decisão

- **Três buckets de mídia** (nomes finais no Terraform de cada dono), sempre **separados**:
  - **Frio `upload`:** vídeo original.
  - **Quente `upload`:** thumbs de catálogo.
  - **Frio `processamento`:** frames e zip.
- **Classe GCP no MVP (demo):** **Standard** nos três buckets. Motivo: a infra sobe e desce **no mesmo dia**; Nearline (mín. 30 dias) e Coldline (mín. 90 dias) cobram early delete ao apagar antes do mínimo — na demo isso custa mais que Standard pró-rata.
- **Produto real (fora do MVP acadêmico):** frio em **Nearline** ou **Coldline** conforme frequência de acesso e retenção real; quente de thumbs permanece **Standard** (ou equivalente de acesso frequente).
- **Front estático** (`web`): bucket/hosting **separado**, sem mídia de domínio.
- **Posse:** só o serviço dono grava/apaga no próprio bucket ([RNF07](../especificacao/03_requisitos-nao-funcionais.md)). `processamento-worker` lê o original só via **URL assinada** emitida por `upload` ([RNF16](../especificacao/03_requisitos-nao-funcionais.md)). Thumb: ao consumir `processamento-video-atualizado` (`iniciado`), `upload` faz **GET** em `processamento-api` (frame válido), baixa e grava no próprio quente — sem URL no barramento e sem escrita cross-bucket.
- **Sem ACL pública** nos buckets de mídia. Download do zip e leitura do original: **URLs assinadas** ([RNF15](../especificacao/03_requisitos-nao-funcionais.md)).
- **Lifecycle de domínio:** zip com retenção de **7 dias** após sucesso ([RF23](../especificacao/02_requisitos-funcionais.md)); original e frames removidos ao concluir o ciclo ([RF19](../especificacao/02_requisitos-funcionais.md)).
- **Chaves determinísticas** no frio de `processamento` (ex. `{videoId}/frames/{indice}.jpg`, `{videoId}/frames.zip`) para retomada idempotente ([ADR 004](004-processamento-checkpoint-retomada.md)).
- **Provisionamento:** cada serviço sobe/desce **os próprios** buckets no TF do repo (demo-up/down do serviço ou orquestração no `e2e`). **Sem** módulo compartilhado `infra-storage`.

## 3. Justificativa

- Buckets separados preservam posse e IAM por domínio.
- Standard na demo evita early delete de Nearline/Coldline no tear-down do mesmo dia.
- Thumb via GET service-to-service evita URL assinada no Pub/Sub e mantém a gravação só em `upload`.

## 4. Alternativas Consideradas

- **Um bucket único com prefixos:** simples; mistura posse, IAM e lifecycle.
- **Módulo `infra-storage` compartilhado:** centraliza; desnecessário com um dono por bucket.
- **Nearline/Coldline no frio já no MVP:** mais barato em armazenamento contínuo; inadequado quando a demo apaga tudo no mesmo dia (early delete).
- **Thumbs escritas por `processamento-worker` no quente de `upload`:** atalho; viola posse cross-domínio.
- **`frameReadUrl` no evento `iniciado`:** conveniente; expõe URL no barramento e acopla validade do link ao Pub/Sub.

## 5. Consequências

### Positivas

- Custo de demo alinhado ao tempo real de existência dos objetos.
- Contratos de leitura entre serviços só via URL assinada (original) ou GET autenticado (frame → thumb).
- TF por serviço espelha a posse.

### Negativas / Riscos (Mitigações)

- **Standard no frio custa mais que Nearline se os buckets ficarem ligados por semanas.**
  - *Mitigação:* demo-up/down no mesmo dia; em produto real migrar frio para Nearline/Coldline.
- **GET de frame depende de `processamento-api` no momento do consume.**
  - *Mitigação:* retry/DLQ do consumer de `upload`; API retorna qualquer frame já confirmado no frio.
