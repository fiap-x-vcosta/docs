# Agregados

Transcrição de [`04_agregados.jpg`](04_agregados.jpg).

## Agregado: Usuário

- **ML:** Tela de Cadastro; Tela de Login  
- **AT:** Usuário  
- **CMD:** Cria Conta na Plataforma; Loga na Plataforma  
- **EV:** Usuário Criado; Usuário Logado  
- **POL:** —

## Agregado: Triagem

- **ML:** Lista de Vídeos / Tela de Upload  
- **AT:** Usuário  
- **CMD:**
  - Faz Upload de Vídeo na Plataforma
  - Faz Triagem do Vídeo Recebido
  - Persiste Vídeo em um Storage Frio
- **EV:**
  - Upload de Vídeo Feito
  - Triagem de Vídeo Concluída
  - Vídeo Rejeitado na Triagem
  - Vídeo Persistido
  - Falha ao Persistir
- **POL:**
  - Fazer Triagem do Vídeo Recebido ← EV Upload de Vídeo Feito
  - Persistir Vídeo em um Storage Frio ← EV Triagem de Vídeo Concluída
  - Notificar Usuário sobre Falha no Processamento do Vídeo ← EV Vídeo Rejeitado na Triagem; EV Falha ao Persistir
  - Enfileirar Vídeo para Processamento ← EV Vídeo Persistido

## Agregado: Processamento

- **ML:** Lista de Vídeos (cancelamento)  
- **AT:** Usuário (cancelamento)  
- **CMD:**
  - Enfileira Vídeo para Processamento
  - Inicia Processamento do Vídeo
  - Cancela Processamento do Vídeo
  - Processa Vídeo
  - Salva .zip com as imagens temporárias
  - Deleta Imagens Temporárias
  - Gera URL de download
- **EV:**
  - Vídeo Enfileirado para Processamento
  - Processamento do Vídeo Iniciado
  - Processamento do Vídeo Cancelado
  - Vídeo Processado com Falha
  - Vídeo Processado com Sucesso
  - Imagens Salvas em .zip
  - Imagens Temporárias Deletadas
  - URL de Download Gerada
- **POL:**
  - Iniciar Processamento do Vídeo ← EV Vídeo Enfileirado para Processamento
  - Processar Vídeo ← EV Processamento do Vídeo Iniciado
  - Deletar Imagens Temporárias ← EV Processamento do Vídeo Cancelado; EV Vídeo Processado com Falha; EV Imagens Salvas em .zip
  - Notificar Usuário sobre Falha no Processamento do Vídeo ← EV Vídeo Processado com Falha *(não* a partir de Cancelado)
  - Gerar .zip com as imagens ← EV Vídeo Processado com Sucesso
  - Gerar URL de Download ← EV Imagens Salvas em .zip *(em paralelo com deletar temp)*
  - Notificar Usuário sobre Sucesso no Processamento do Vídeo ← EV URL de Download Gerada

## Agregado: Notificação

- **CMD:** Notifica o Usuário de Sucesso; Notifica o Usuário de Falha  
- **EV:** Notificação de Sucesso Enviada; Notificação de Falha Enviada  
- **POL:** entradas Notificar … Sucesso / Notificar … Falha conforme políticas dos outros agregados  
