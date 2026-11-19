# Linguagem onipresente

Vocabulário compartilhado do FIAP X. Alinhado ao [event storming](../event-storming/).

## Atores e interfaces

| Termo | Significado |
|-------|-------------|
| **Usuário** | Pessoa que se cadastra, autentica e opera vídeos na plataforma. |
| **Tela de Cadastro** | Interface para criar conta. |
| **Tela de Login** | Interface para autenticar. |
| **Lista de Vídeos / Tela de Upload** | Interface para enviar vídeo, acompanhar status e cancelar processamento em andamento. |

## Agregados (bounded contexts de domínio)

| Agregado | Responsabilidade |
|----------|------------------|
| **Usuário** | Identidade e autenticação (criar conta, logar). |
| **Triagem** | Receber o upload, validar o vídeo e persistir o arquivo no storage frio. |
| **Processamento** | Fila, execução (extração de frames), zip, limpeza de temporários e geração de URL de download. |
| **Notificação** | Envio de e-mail de sucesso ou de falha. |

## Conceitos de mídia e processamento

| Termo | Significado |
|-------|-------------|
| **Vídeo** | Arquivo de mídia enviado pelo usuário. Na **Triagem** ele é validado e, se aceito, persistido. O vínculo com o usuário dono define quem pode baixar o resultado. |
| **Processamento** (conceito) | Unidade de trabalho sobre um vídeo já persistido: inicia após o enfileiramento, pode concluir com sucesso, falhar ou ser cancelado pelo usuário. Não é o mesmo que o arquivo de vídeo; é o ciclo de vida da execução. |
| **Triagem** (conceito) | Validação do vídeo recebido (aceitação ou rejeição) antes da persistência definitiva. |
| **Storage frio** | Armazenamento durável e de menor custo de acesso frequente para o vídeo original (e, no desenho de mídia, frames/zip conforme ADRs de storage). |
| **Imagens temporárias** | Frames (e correlatos) gerados durante o processamento, mantidos de forma recuperável até o zip e/ou limpeza. |
| **Thumb** | Miniatura para UX. No storm atual **não** há evento de domínio de thumbs; a anotação no sucesso (“Thumbs geradas, imagens temporárias gravadas”) é observação de implementação, não termo de evento. |
| **Zip** | Arquivo `.zip` com as imagens extraídas do vídeo, disponível para download após o sucesso. |
| **URL de download** | Link (tipicamente assinado/temporário) que permite baixar o zip. Somente o usuário que enviou o vídeo pode obtê-la. |
| **Progresso** | Indicação de andamento do processamento (ex. frames feitos / total). **Não** é evento de domínio no storm; é dado de leitura/consulta. |

## Notificação

| Termo | Significado |
|-------|-------------|
| **Notificação de sucesso** | E-mail informado ao usuário quando o fluxo feliz chega à URL de download. |
| **Notificação de falha** | E-mail informado ao usuário após rejeição na triagem, falha ao persistir ou falha no processamento. **Cancelamento** pelo usuário **não** dispara notificação. |

## Posse (metadados)

Interpretação a partir dos agregados do storm (a detalhar no modelo de dados):

- **Triagem** é dona do fato “vídeo recebido / aceito / rejeitado / persistido” e do vínculo com o usuário.
- **Processamento** é dono do ciclo de execução (enfileirado → iniciado → sucesso/falha/cancelado), zip, temporários e URL.
- **Notificação** é dona do envio do e-mail (sucesso/falha).
- Consultas ao usuário (listagem/status/progresso/download) agregam esses fatos via a API pública do sistema (BFF), sem misturar stores entre agregados.

## Termos evitados

- Não usar naming que combine o nome do produto com “videos” em camel/kebab/snake.
- Preferir `fiap-x`, `processamento`, `midia`, `frames`, `triagem`.
