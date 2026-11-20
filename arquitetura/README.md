# Arquitetura (C4)

C4 com sprites oficiais ([C4-PlantUML](https://github.com/plantuml-stdlib/C4-PlantUML)): `.puml` é a fonte; `.svg` é o render versionado (layout alinhado). Os `.md` só embutem o SVG.

| Visão | Fonte | Render |
|-------|--------|--------|
| Contexto (L1) | [01_contexto.puml](01_contexto.puml) | [01_contexto.svg](01_contexto.svg) |
| Containers (L2) | [02_containers.puml](02_containers.puml) | [02_containers.svg](02_containers.svg) |
| Componentes — Upload | [03_componentes-upload.puml](03_componentes-upload.puml) | [03_componentes-upload.svg](03_componentes-upload.svg) |
| Componentes — Processamento | [04_componentes-processamento.puml](04_componentes-processamento.puml) | [04_componentes-processamento.svg](04_componentes-processamento.svg) |
| Componentes — BFF | [05_componentes-bff.puml](05_componentes-bff.puml) | [05_componentes-bff.svg](05_componentes-bff.svg) |

Regenerar SVG (Docker):

```bash
docker run --rm -u "$(id -u):$(id -g)" -v "$PWD:/data" plantuml/plantuml:1.2024.8 \
  -tsvg -charset UTF-8 \
  /data/01_contexto.puml /data/02_containers.puml /data/03_componentes-upload.puml \
  /data/04_componentes-processamento.puml /data/05_componentes-bff.puml
```
