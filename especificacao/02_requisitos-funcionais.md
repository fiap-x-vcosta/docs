# Requisitos funcionais

Glossário: [linguagem onipresente](01_linguagem-onipresente.md). Limites de mídia: [limites de mídia](../requisitos/limites-de-midia.md).

## 19 de novembro de 2026

### Autenticação

- **RF01** — O sistema permite **criar conta** com usuário e senha.
- **RF02** — O sistema permite **login** com usuário e senha e emite credencial de sessão (JWT).
- **RF03** — Operações de upload, listagem, cancelamento e download exigem usuário autenticado.

### Upload e triagem

- **RF04** — O usuário autenticado pode **enviar um vídeo** pela lista/upload.
- **RF05** — Após o upload, o sistema executa **triagem**. Se rejeitado, o usuário recebe **notificação de falha** por e-mail.
- **RF06** — Se a triagem concluir com sucesso, o sistema **persiste** o vídeo no armazenamento frio.
- **RF07** — Se a persistência falhar, o usuário recebe **notificação de falha** por e-mail.

### Processamento

- **RF08** — Vídeo persistido é **enfileirado** e o processamento é iniciado de forma assíncrona.
- **RF09** — O sistema processa **mais de um vídeo ao mesmo tempo** (paralelismo entre processamentos).
- **RF10** — Em picos, requisições/aceites de processamento **não são perdidos** (mensageria durável).
- **RF11** — Processamento bem-sucedido gera **frames**, empacota em **`.zip`**, remove os frames temporários e gera **URL de download** (limpeza e URL podem ocorrer em paralelo após o zip).
- **RF12** — Em **falha** no processamento, o sistema remove frames temporários (se houver) e envia **notificação de falha** por e-mail.
- **RF13** — O usuário pode **cancelar** um processamento em andamento; o sistema remove frames temporários e **não** envia e-mail de notificação.

### Consulta, download e notificação de sucesso

- **RF14** — O usuário vê a **listagem/status** dos seus vídeos/processamentos (incluindo progresso quando em andamento).
- **RF15** — Após sucesso (URL gerada), o sistema envia **notificação de sucesso** por e-mail.
- **RF16** — **Somente o usuário que enviou** o vídeo pode obter a URL e baixar o `.zip`.
- **RF17** — O download do resultado é feito a partir da **URL de download** gerada pelo sistema.
