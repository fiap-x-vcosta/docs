# Linha do tempo (completa)

Transcrição de [`03_linha-do-tempo.jpg`](03_linha-do-tempo.jpg).

## Feliz

Usuário (Tela de Cadastro)  
Cria Conta na Plataforma > Usuário Criado

Usuário (Tela de Login)  
Loga na Plataforma > Usuário Logado

Usuário (Lista de Vídeos / Tela de Upload)  
Faz Upload de Vídeo na Plataforma > Upload de Vídeo Feito

Upload de Vídeo Feito (Fazer Triagem do Vídeo Recebido)  
Faz Triagem do Vídeo Recebido > Triagem de Vídeo Concluída

Triagem de Vídeo Concluída (Persistir Vídeo em um Storage Frio)  
Persiste Vídeo em um Storage Frio > Vídeo Persistido

Vídeo Persistido (Enfileirar Vídeo para Processamento)  
Enfileira Vídeo para Processamento > Vídeo Enfileirado para Processamento

Vídeo Enfileirado para Processamento (Iniciar Processamento do Vídeo)  
Inicia Processamento do Vídeo > Processamento do Vídeo Iniciado

Processamento do Vídeo Iniciado (Processar Vídeo)  
Processa Vídeo > Vídeo Processado com Sucesso

Vídeo Processado com Sucesso (Gerar .zip com as imagens)  
Salva .zip com as imagens temporárias > Imagens Salvas em .zip

Imagens Salvas em .zip (Deletar Imagens Temporárias) — *em paralelo com URL*  
Deleta Imagens Temporárias > Imagens Temporárias Deletadas

Imagens Salvas em .zip (Gerar URL de Download) — *em paralelo com delete temp*  
Gera URL de download > URL de Download Gerada

URL de Download Gerada (Notificar Usuário sobre Sucesso no Processamento do Vídeo)  
Notifica o Usuário de Sucesso > Notificação de Sucesso Enviada

## Exceção

Upload de Vídeo Feito (Fazer Triagem do Vídeo Recebido)  
Faz Triagem do Vídeo Recebido > Vídeo Rejeitado na Triagem

Vídeo Rejeitado na Triagem (Notificar Usuário sobre Falha no Processamento do Vídeo)  
Notifica o Usuário de Falha > Notificação de Falha Enviada

Triagem de Vídeo Concluída (Persistir Vídeo em um Storage Frio)  
Persiste Vídeo em um Storage Frio > Falha ao Persistir

Falha ao Persistir (Notificar Usuário sobre Falha no Processamento do Vídeo)  
Notifica o Usuário de Falha > Notificação de Falha Enviada

Processamento do Vídeo Iniciado (Processar Vídeo)  
Processa Vídeo > Vídeo Processado com Falha

Vídeo Processado com Falha (Deletar Imagens Temporárias)  
Deleta Imagens Temporárias > Imagens Temporárias Deletadas

Vídeo Processado com Falha (Notificar Usuário sobre Falha no Processamento do Vídeo)  
Notifica o Usuário de Falha > Notificação de Falha Enviada

Usuário (Lista de Vídeos)  
Cancela Processamento do Vídeo > Processamento do Vídeo Cancelado

Processamento do Vídeo Cancelado (Deletar Imagens Temporárias)  
Deleta Imagens Temporárias > Imagens Temporárias Deletadas  
*(cancelamento não gera notificação)*
