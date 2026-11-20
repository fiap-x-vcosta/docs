# Arquitetura (C4)

C4 com sprites oficiais ([C4-PlantUML](https://github.com/plantuml-stdlib/C4-PlantUML)): `.puml` é a fonte; `.svg` é o render versionado.

| # | Visão | Fonte | Render |
|---|--------|--------|--------|
| 01 | Contexto (L1) | [01_contexto.puml](01_contexto.puml) | [01_contexto.svg](01_contexto.svg) |
| 02 | Containers — borda pública | [02_containers-publico.puml](02_containers-publico.puml) | [02_containers-publico.svg](02_containers-publico.svg) |
| 03 | Containers — upload | [03_containers-upload.puml](03_containers-upload.puml) | [03_containers-upload.svg](03_containers-upload.svg) |
| 04 | Containers — processamento | [04_containers-processamento.puml](04_containers-processamento.puml) | [04_containers-processamento.svg](04_containers-processamento.svg) |
| 05 | Containers — notificacao | [05_containers-notificacao.puml](05_containers-notificacao.puml) | [05_containers-notificacao.svg](05_containers-notificacao.svg) |
| 06 | Componentes — upload | [06_componentes-upload.puml](06_componentes-upload.puml) | [06_componentes-upload.svg](06_componentes-upload.svg) |
| 07 | Componentes — consulta progresso | [07_componentes-processamento-consulta.puml](07_componentes-processamento-consulta.puml) | [07_componentes-processamento-consulta.svg](07_componentes-processamento-consulta.svg) |
| 08 | Componentes — cancelamento | [08_componentes-processamento-cancelamento.puml](08_componentes-processamento-cancelamento.puml) | [08_componentes-processamento-cancelamento.svg](08_componentes-processamento-cancelamento.svg) |
| 09 | Componentes — bff | [09_componentes-bff.puml](09_componentes-bff.puml) | [09_componentes-bff.svg](09_componentes-bff.svg) |

Regenerar SVG:

```bash
docker run --rm -u "$(id -u):$(id -g)" -e HOME=/tmp -v "$PWD:/data" plantuml/plantuml:1.2024.8 \
  -tsvg -charset UTF-8 /data/*.puml
```
