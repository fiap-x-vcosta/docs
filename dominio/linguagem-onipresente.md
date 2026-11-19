# Linguagem onipresente

- **Usuário:** pessoa que se cadastra, autentica e faz upload de vídeo na plataforma.
- **Vídeo:** arquivo de mídia enviado pelo usuário; após aceito, é gravado no armazenamento frio. O vínculo com o usuário dono define quem pode baixar o resultado.
- **Triagem:** validação do vídeo recebido (aceitação ou rejeição) antes da persistência definitiva.
- **Processamento:** ciclo de trabalho sobre um vídeo já persistido (enfileirar, iniciar, concluir com sucesso, falhar ou cancelar).
- **Armazenamento frio:** armazenamento durável de menor custo de acesso frequente para o vídeo original (e demais objetos de mídia conforme ADRs de storage).
- **Armazenamento quente:** armazenamento de acesso frequente para thumbs e demais objetos que a interface precisa servir com baixa latência.
- **Frame:** imagem extraída do vídeo durante o processamento, mantida de forma recuperável até o zip e/ou a limpeza.
- **Thumb:** miniatura para exibição na interface.
- **Zip:** arquivo `.zip` com as imagens extraídas do vídeo, disponível para download após o sucesso.
- **URL de download:** link que permite baixar o zip; somente o usuário que enviou o vídeo pode obtê-la.
- **Progresso:** indicação de andamento do processamento (ex. frames feitos / total).
- **Notificação de sucesso:** e-mail ao usuário quando a URL de download está disponível.
- **Notificação de falha:** e-mail ao usuário após rejeição na triagem, falha ao persistir ou falha no processamento. Cancelamento pelo usuário não dispara notificação.
