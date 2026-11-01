# Plantillas

📍 [InnLab](../README.md) › Referencia › **Plantillas**

> **Resumen:** textos listos para copiar. Las de PR e issue se guardan en `.github/` de cada repo, para que GitHub las cargue solas.

## Pull Request

Archivo: `.github/pull_request_template.md`

```markdown
## Qué cambia
<!-- 1-3 líneas -->

## Por qué
<!-- Contexto o problema que resuelve -->

## Cómo se probó
<!-- Tests agregados y pasos manuales. En frontend, capturas de escritorio y móvil si hay cambio visual -->

## Cómo validarlo en staging
<!-- Pasos para que quien revisa pruebe en staging todo lo que dice "Qué cambia" -->

## Issue
Fixes #

## Capturas
<!-- Solo si hay cambio visual: antes / después -->

## Checklist
- [ ] Corre en local y pasa lint + tests + build
- [ ] Tests para la lógica nueva o modificada, sin bajar la cobertura mínima
- [ ] CI en verde (hoy no hay protección de ramas: lo comprueba quien revisa)
- [ ] Versión subida y entrada en `CHANGELOG.md`
- [ ] Sin secretos, datos personales ni URLs hardcodeadas
- [ ] README / .env.example / wiki actualizados si aplica
- [ ] **Validado en staging por quien revisa** (cuando exista el ambiente)
- [ ] Backend: Quality Gate de SonarQube Cloud en verde en este PR (check `sonarqube`)
```

## Issue

Archivos: `.github/ISSUE_TEMPLATE/bug.md` y `.github/ISSUE_TEMPLATE/feature.md`

```markdown
---
name: Bug
labels: bug
---
## Qué pasa
## Qué debería pasar
## Pasos para reproducir
1.
## Ambiente
local / staging / producción — navegador / versión
```

```markdown
---
name: Feature
labels: feature
---
## Qué se necesita
## Para quién y por qué
## Criterios de aceptación
- [ ]
```

## README corto de repo

La wiki es la fuente de verdad: el README solo sirve para arrancar y enlaza a ella.

```markdown
# <nombre-del-repo>

<Qué es en 1-2 líneas>. Parte del proyecto **<Proyecto>** de InnLab.

📖 **Documentación completa y estándares:** <enlace a la página del repo en Notion>

## Arranque rápido
    <gestor> install
    cp .env.example .env   # los valores los da el responsable del proyecto
    <gestor> run dev

## Tests
    <gestor> test
```

## ADR

Archivo: `docs/adr/NNNN-titulo-corto.md`

```markdown
# NNNN. <Título de la decisión>

- Fecha: AAAA-MM-DD
- Estado: propuesta | aceptada | reemplazada por NNNN

## Contexto
<Qué problema o fuerza obliga a decidir>

## Decisión
<Qué se decidió>

## Alternativas consideradas
- <Opción A>: por qué no
- <Opción B>: por qué no

## Consecuencias
<Qué se gana, qué se pierde, qué hay que hacer ahora>
```
