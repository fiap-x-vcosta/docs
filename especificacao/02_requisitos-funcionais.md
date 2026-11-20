# Requisitos funcionais

Glossário: [linguagem onipresente](01_linguagem-onipresente.md).

## 19 de novembro de 2026

### Autenticação

- **RF01** — O sistema permite **criar conta** com usuário e senha.
- **RF02** — O sistema permite **login** com usuário e senha e emite credencial de sessão (JWT).
- **RF03** — Operações de upload, listagem, cancelamento e download exigem usuário autenticado.

### Upload e triagem

- **RF04** — O usuário autenticado pode **enviar um vídeo** pela lista/upload.
- **RF05** — Após o upload, o sistema executa **triagem**. Se rejeitado, o usuário recebe **notificação de falha** por e-mail **com o motivo**.
- **RF06** — Se a triagem concluir com sucesso, o sistema **persiste** o vídeo no armazenamento frio.
- **RF07** — Se a persistência falhar, o usuário recebe **notificação de falha** por e-mail **com o motivo**.

### Processamento

- **RF08** — Vídeo persistido é **enfileirado** e o processamento é iniciado de forma assíncrona.
- **RF09** — O sistema processa **mais de um vídeo ao mesmo tempo** (paralelismo entre processamentos).
- **RF10** — Em picos, requisições/aceites de processamento **não são perdidos** (mensageria durável).
- **RF11** — Processamento bem-sucedido gera **frames**, empacota em **`.zip`** e gera **URL de download** (zip e URL; limpeza de frames conforme RF19).
- **RF12** — Em **falha** no processamento, o sistema registra o **motivo da falha** e envia **notificação de falha** por e-mail **com o motivo**.
- **RF13** — O usuário pode **cancelar** somente enquanto o status for **enfileirado** ou **processando**; após cancelamento solicitado, sucesso, falha, cancelado, rejeitado na triagem ou falha ao persistir, o cancelamento **não** é permitido. Cancelamento **não** envia e-mail.
- **RF14** — A listagem/histórico do usuário é paginada a partir do **Upload** (projeção de status) e inclui **todos os status** do ciclo ([status do video na listagem](01_linguagem-onipresente.md#status-do-video-na-listagem)); com progresso quando `processando` (consulta ao Processamento) e **motivo** nos status de rejeição/falha.
- **RF15** — Durante **processando**, a listagem exibe **thumb** quando o Upload já tiver gravado a miniatura a partir de `processamento-video-status` (`em-processamento`).
- **RF16** — Após sucesso (URL gerada), o sistema envia **notificação de sucesso** por e-mail.
- **RF17** — **Somente o usuário que enviou** o vídeo (dono) pode obter a URL de download e baixar o `.zip` desse vídeo.
- **RF18** — O download do resultado é feito a partir da **URL de download** gerada pelo sistema (assinada; [RNF15](03_requisitos-nao-funcionais.md)).
- **RF19** — Ao concluir o ciclo com **sucesso**, **falha** ou **cancelamento**, o **Processamento** remove **frames** no frio que possui; o **Upload**, ao consumir `processamento-video-status` (`sucesso` / `falha` / `cancelado`), remove o **vídeo original** no frio e, em **falha** ou **cancelamento**, a **thumb** no quente ([RNF07](03_requisitos-nao-funcionais.md), [RNF17](03_requisitos-nao-funcionais.md)). Em **sucesso**, a thumb de catálogo permanece no Upload.
- **RF20** — O usuário **não** pode reenviar nem solicitar reprocessamento do mesmo vídeo após falha ou rejeição; recuperação de falha transitória é responsabilidade do próprio serviço.

### Limites de mídia e retenção

- **RF21** — Na **triagem**, o sistema rejeita vídeos que não atendam aos **formatos**, **tamanho máximo** e **duração máxima** aceitos (valores nesta seção).
- **RF22** — O processamento extrai frames segundo a **amostragem** definida nesta seção.
- **RF23** — Após o sucesso, o **zip** permanece disponível conforme a **retenção** desta seção. Enquanto o zip existir, o dono pode obter **URL de download** válida.
- **RF24** — Não há **limite por usuário** (quota de quantidade ou volume) no MVP.

#### Valores (MVP)

- **Formatos aceitos:** qualquer formato de vídeo que o **FFmpeg** consiga demuxar/decodificar; demais são rejeitados na triagem.
- **Tamanho máximo do vídeo:** **1 GB**.
- **Duração máxima do vídeo:** **20 minutos**.
- **Amostragem de frames:** **1 frame a cada 30 frames** da fonte (em vídeo 30 fps ≈ 1 frame por segundo).
- **Retenção do zip:** **7 dias** após o sucesso; depois o objeto é removido.
- **Validade da URL de download:** alinhada à existência do zip (enquanto o objeto for válido, o sistema fornece URL assinada utilizável).
- **Vídeo original:** removido pelo **Upload** ao concluir o ciclo; **frames** removidos pelo **Processamento**; **thumb** removida pelo **Upload** em falha/cancelamento (RF19).
- **Limite por usuário:** **nenhum** (RF24).
