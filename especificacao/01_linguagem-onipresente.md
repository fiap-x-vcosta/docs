# Linguagem onipresente

- **Usuário:** pessoa que se cadastra, autentica e faz upload de vídeo na plataforma.
- **Vídeo:** arquivo de mídia enviado pelo usuário; após aceito, é gravado no frio do Upload e removido pelo Upload ao concluir o ciclo. O vínculo com o usuário dono define quem pode baixar o resultado.
- **Triagem:** validação do vídeo recebido (aceitação ou rejeição) antes da persistência definitiva.
- **Processamento:** ciclo de trabalho sobre um vídeo já persistido (enfileirar, iniciar, concluir com sucesso, falhar ou cancelar).
- **Armazenamento frio:** armazenamento durável de menor custo de acesso frequente para o vídeo original (e demais objetos de mídia conforme ADRs de storage).
- **Armazenamento quente:** armazenamento de acesso frequente no Upload para thumbs de catálogo/listagem.
- **Frame:** imagem extraída do vídeo durante o processamento, mantida de forma recuperável no frio do Processamento até o zip e/ou a limpeza.
- **Thumb:** miniatura de catálogo no quente do Upload; atualizada a partir de frame produzido no Processamento (via evento/API).
- **Zip:** arquivo `.zip` com as imagens extraídas do vídeo; após o sucesso permanece disponível pelo período de retenção (7 dias no MVP).
- **URL de download:** URL assinada que permite baixar o zip; somente o dono pode obtê-la; permanece obtível enquanto o zip existir.
- **Progresso:** indicação de andamento do processamento (ex. frames feitos / total); valor canônico no serviço dono, com eventual cache no BFF.

- **Notificação de sucesso:** e-mail ao usuário quando a URL de download está disponível.
- **Notificação de falha:** e-mail ao usuário após rejeição na triagem, falha ao persistir ou falha no processamento; inclui o **motivo da falha**. Cancelamento pelo usuário não dispara notificação.
- **Motivo de falha:** informação registrada, exibida na listagem/histórico e incluída na notificação de falha quando o ciclo termina com rejeição ou falha.

## Status do processamento

Status visíveis na listagem/histórico do usuário:

- **Em triagem:** vídeo enviado; triagem em curso ou pendente.
- **Rejeitado na triagem:** triagem recusou o vídeo.
- **Falha ao persistir:** triagem aceitou, mas a gravação no armazenamento frio falhou.
- **Enfileirado:** vídeo persistido e aguardando início do processamento.
- **Processando:** processamento iniciado (extração de frames em andamento).
- **Sucesso:** zip gerado e URL de download disponível.
- **Falha:** processamento terminou com erro (motivo registrado).
- **Cancelado:** usuário cancelou o processamento em andamento.
