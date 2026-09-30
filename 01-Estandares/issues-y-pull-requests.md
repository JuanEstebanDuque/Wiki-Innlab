# Issues y Pull Requests

📍 [InnLab](../README.md) › [Estándares](README.md) › **Issues y Pull Requests**

> **Resumen:** el trabajo parte de un issue, se desarrolla en una rama propia y entra por un PR con plantilla, 1 aprobación y squash merge. El PR cierra el issue con `Fixes #n`.

## Flujo completo

1. **Issue.** Se crea o se toma un issue en el repo. Describe el problema o la propuesta y cómo se sabe que quedó resuelto (criterio de aceptación).
2. **Rama.** `git checkout dev && git pull && git checkout -b feat/42-short-description` (ver [Git y ramas](git-y-ramas.md)).
3. **Commits.** Pequeños y con el formato de [Commits](commits.md).
4. **Push y PR hacia `dev`**, usando la plantilla de abajo.
5. **Review.** Mínimo 1 aprobación de alguien distinto al autor.
6. **Merge con "Squash and merge"**, que deja un solo commit limpio en `dev`. El título del PR se convierte en ese commit, así que también sigue el formato de commits.
7. **Limpieza.** Se borra la rama y se hace `git fetch --prune` en local.

**Excepción:** los PRs de release (`dev` → `main`) usan **merge commit** y no squash, para conservar la historia.

## Issues

- Título corto y accionable: `Export report as PDF`, no `PDF`.
- Etiquetas mínimas: `bug`, `feature`, `tech-debt`, `docs`.
- Los ítems del plan de remediación de cada repo se convierten en issues con la etiqueta `tech-debt`.
- Plantilla: [Plantillas → Issue](../06-Referencia/plantillas.md).

## Plantilla de PR

Se guarda en cada repo como `.github/pull_request_template.md`. Texto completo en [Plantillas](../06-Referencia/plantillas.md).

```markdown
## Qué cambia
## Por qué
## Cómo se probó
## Issue
Fixes #
## Capturas (si hay cambio visual)
## Checklist
- [ ] Corre en local y pasa lint + tests
- [ ] Tests para la lógica nueva
- [ ] Sin secretos ni datos personales
- [ ] README / wiki actualizados si aplica
```

## Tamaño

- Un PR hace **una** cosa. Idealmente tiene menos de ~400 líneas cambiadas sin contar lockfiles.
- Las actualizaciones grandes de dependencias van en su propio PR.

## Checklist del revisor

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
