# Requisitos funcionais e não funcionais

Fonte: enunciado do hackathon FIAP X + [event storming](../event-storming/) + decisões de produto já refletidas neste repositório. Glossário: [linguagem onipresente](../dominio/linguagem-onipresente.md).

Limites numéricos de mídia (tamanho, duração, amostragem) ficam em [limites de mídia](limites-de-midia.md) quando fechados.

## Requisitos funcionais

### Autenticação

- **RF01** — O sistema permite **criar conta** com usuário e senha.
- **RF02** — O sistema permite **login** com usuário e senha e emite credencial de sessão (JWT).
- **RF03** — Operações de upload, listagem, cancelamento e download exigem usuário autenticado.

### Upload e triagem

- **RF04** — O usuário autenticado pode **enviar um vídeo** pela lista/upload.
- **RF05** — Após o upload, o sistema executa **triagem**. Se rejeitado, o usuário recebe **notificação de falha** por e-mail.
- **RF06** — Se a triagem concluir com sucesso, o sistema **persiste** o vídeo no storage frio.
- **RF07** — Se a persistência falhar, o usuário recebe **notificação de falha** por e-mail.

### Processamento

- **RF08** — Vídeo persistido é **enfileirado** e o processamento é iniciado de forma assíncrona.
- **RF09** — O sistema processa **mais de um vídeo ao mesmo tempo** (paralelismo entre processamentos).
- **RF10** — Em picos, requisições/aceites de processamento **não são perdidos** (mensageria durável).
- **RF11** — Processamento bem-sucedido gera **imagens**, empacota em **`.zip`**, remove **imagens temporárias** e gera **URL de download** (delete temp e URL podem ocorrer em paralelo após o zip).
- **RF12** — Em **falha** no processamento, o sistema remove imagens temporárias (se houver) e envia **notificação de falha** por e-mail.
- **RF13** — O usuário pode **cancelar** um processamento em andamento; o sistema remove imagens temporárias e **não** envia e-mail de notificação.

### Consulta, download e notificação de sucesso

- **RF14** — O usuário vê a **listagem/status** dos seus vídeos/processamentos (incluindo progresso quando em andamento).
- **RF15** — Após sucesso (URL gerada), o sistema envia **notificação de sucesso** por e-mail.
- **RF16** — **Somente o usuário que enviou** o vídeo pode obter a URL e baixar o `.zip`.
- **RF17** — O download do resultado é feito a partir da **URL de download** gerada pelo sistema.

## Requisitos não funcionais

### Arquitetura e persistência

- **RNF01** — Arquitetura em **microsserviços**, alinhada aos agregados Usuário, Triagem, Processamento e Notificação (mais a face pública de consultas/BFF).
- **RNF02** — Dados de domínio **persistidos** (PostgreSQL por serviço dono, conforme ADRs).
- **RNF03** — Mídia em **object storage multi-tier** (frio para vídeo/frames/zip; quente para thumbs de UX — detalhe em ADR de storage).
- **RNF04** — Única **API HTTP pública** de produto é o **BFF**; demais serviços apenas rede interna. Front estático é superfície pública separada (HTML/JS).
- **RNF05** — Autenticação com JWT **RS256** e **JWKS**; chave privada só no serviço de autenticação.

### Mensageria e resiliência

- **RNF06** — Integração assíncrona via **Google Pub/Sub** (um tópico por evento de domínio, conforme catálogo).
- **RNF07** — Publicação de eventos de integração com **transactional outbox**; consumidores **idempotentes**.
- **RNF08** — Worker de processamento com **checkpoint/retomada** (estado de controle + objetos de mídia duráveis), para não perder trabalho em falha de pod.

### Qualidade, entrega e operação

- **RNF09** — Código versionado no **GitHub** (org do projeto); mudanças via PR.
- **RNF10** — Testes automatizados; cobertura unitária ≥ **80%** line+branch por assembly (.NET).
- **RNF11** — **CI** automático (lint, testes, Sonar); provisionamento/deploy caros na GCP são **manuais** (janela de demo).
- **RNF12** — Observabilidade com **Datadog** (traces atravessando HTTP e Pub/Sub).
- **RNF13** — E-mail via **SendGrid**.
- **RNF14** — Progresso de processamento é **dado de leitura** (não evento de domínio); o BFF não compartilha banco com outros serviços (cache próprio permitido).

## Fora do escopo do MVP (evoluções)

Itens conscientes deixados para depois (não bloqueiam o MVP documentado aqui), entre outros: limites por usuário; prioridade de fila; retenção configurável do zip; qualidade/tipo de imagem escolhível; compartilhamento do zip com outros usuários; WebSocket de progresso; preferência fina de notificação; eventos “notificação solicitada” separados do envio.
