# Issues y Pull Requests

📍 [InnLab](../README.md) › [Estándares](README.md) › **Issues y Pull Requests**

> **Resumen:** el trabajo parte de un issue, se desarrolla en una rama propia y entra por un PR con plantilla. Quien revisa comprueba que el CI esté en verde, **valida el cambio en staging** y, si todo está bien, aprueba y hace squash merge; desde ahí el paso a producción es automático. El PR cierra el issue con `Fixes #n`.

## Flujo completo

1. **Issue.** Se crea o se toma un issue en el repo. Describe el problema o la propuesta y cómo se sabe que quedó resuelto (criterio de aceptación).
2. **Rama.** `git checkout dev && git pull && git checkout -b feat/42-short-description` (ver [Git y ramas](git-y-ramas.md)).
3. **Commits.** Pequeños y con el formato de [Commits](commits.md).
4. **Push y PR hacia `dev`**, usando la plantilla de abajo.
5. **Review.** Mínimo 1 aprobación de alguien distinto al autor. Quien revisa:
   - comprueba que **todos los checks del CI estén en verde** (hoy no hay protección de ramas, así que nada más lo impide; ver [CI y calidad](ci-y-calidad.md#sin-protección-de-ramas-situación-actual));
   - **prueba el cambio en staging**, siguiendo "Cómo validarlo en staging" del PR, y deja constancia en un comentario (ver [Despliegue y CI/CD](despliegue-y-ambientes.md)).

   **Nadie fusiona su propio PR.**
6. **Merge con "Squash and merge"**, que deja un solo commit limpio en `dev`. El título del PR se convierte en ese commit, así que también sigue el formato de commits.
7. **Limpieza.** Se borra la rama y se hace `git fetch --prune` en local.

**Después del merge**, la promoción `dev` → `main` y el despliegue a producción son automáticos y usan **merge commit**, para conservar la historia (ver [Versionado y releases](versionado-y-releases.md)).

## Issues

- Título corto y accionable: `Export report as PDF`, no `PDF`.
- Etiquetas mínimas: `bug`, `feature`, `tech-debt`, `docs`.
- Los ítems del plan de remediación de cada repo se convierten en issues con la etiqueta `tech-debt`.
- Plantilla: [Plantillas → Issue](../06-Referencia/plantillas.md).

## Plantilla de PR

Se guarda en cada repo como `.github/pull_request_template.md`. Texto completo en [Plantillas](../06-Referencia/plantillas.md).

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
- [ ] Backend: después del merge, Quality Gate de SonarQube Cloud en verde sobre `main`
```

## Tamaño

- Un PR hace **una** cosa. Idealmente tiene menos de ~400 líneas cambiadas sin contar lockfiles.
- Las actualizaciones grandes de dependencias van en su propio PR.

## Checklist del revisor

- [ ] **Todos los checks del CI están en verde.** Si hay uno en rojo, no se aprueba.
- [ ] **Lo probé en staging** y funciona todo lo que dice "Qué cambia" (cuando exista el ambiente).
- [ ] Trae la versión subida y su entrada en `CHANGELOG.md`.
- [ ] El PR hace lo que dice su descripción, y nada más.
- [ ] Respeta la [nomenclatura](idioma-y-nomenclatura.md) y la estructura de su [stack](../04-Stacks/README.md).
- [ ] Tiene tests para la lógica nueva o modificada, o justifica por qué no aplica.
- [ ] No introduce secretos, credenciales ni URLs de producción hardcodeadas.
- [ ] No sube archivos de asistentes de IA (`CLAUDE.md`, `.claude/`, specs generados).
- [ ] Las funciones y componentes públicos nuevos o modificados tienen su [comentario de documentación](documentacion-en-codigo.md), y está al día.
- [ ] Si mueve archivos, el PR es solo de `refactor:` y no mezcla cambios de lógica ([Estructura](estructura-de-carpetas.md)).
- [ ] No deja `console.log` o `print` de depuración, ni código comentado sin explicación.
- [ ] Los errores se manejan explícitamente (no hay `catch` vacíos).
- [ ] Si cambia cómo se instala o se corre el repo, el README está actualizado.

**Cómo comentar:** se critica el código, no a la persona. Para distinguir lo obligatorio de lo opcional se usa el prefijo `nit:` en lo opcional y `blocking:` en lo que impide el merge.
