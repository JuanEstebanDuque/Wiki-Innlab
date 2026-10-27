# Estándares

📍 [InnLab](../README.md) › **Estándares**

Reglas que aplican a **todos los proyectos y repos** de InnLab. Cada card es independiente: puedes leer solo la que necesitas.

- **Regulan la forma de trabajar, no la tecnología.** Lo que depende del stack está en [Stacks](../04-Stacks/README.md).
- Lo marcado **(Propuesta)** o **(En definición)** todavía no está cerrado. Ver [Pendientes por concretar](../pendientes-por-concretar.md).
- Testing tiene su propia sección: [Testing](../02-Testing/README.md).

| Card | En una línea |
| --- | --- |
| [Git y ramas](git-y-ramas.md) | `main` + `dev`; ramas `<tipo>/<issue>-<descripcion>`; se borran al hacer merge |
| [Commits](commits.md) | `<tipo>(alcance): resumen en inglés, imperativo, ≤72 caracteres` |
| [Versionado y releases](versionado-y-releases.md) | SemVer por repo; cada PR trae su versión y CHANGELOG; la promoción etiqueta `vX.Y.Z` |
| [Issues y Pull Requests](issues-y-pull-requests.md) | Todo parte de un issue; PR con plantilla, 1 aprobación y squash merge |
| [Estructura de carpetas](estructura-de-carpetas.md) | Estructura base por dominio para back y front; obligatoria en repos nuevos, gradual en los existentes |
| [Documentación en el código](documentacion-en-codigo.md) | Toda función o componente público lleva un comentario de qué hace y para qué sirve |
| [Idioma y nomenclatura](idioma-y-nomenclatura.md) | Código en inglés, docs en español; tabla de convenciones de nombres |
| [Seguridad, secretos y accesos](seguridad-y-accesos.md) | Nada de secretos en git; cada cuenta tiene un dueño |
| [Documentación](documentacion.md) | La wiki es la fuente de verdad; README corto que enlaza aquí |
| [CI y calidad](ci-y-calidad.md) | GitHub Actions: lint sin advertencias, build, cobertura mínima, auditoría y SonarQube en backends |
| [Despliegue y CI/CD](despliegue-y-ambientes.md) | PR → staging (validación manual) → producción automática; rollback solo en producción |
| [Dependencias y herramientas](dependencias-y-herramientas.md) | Versiones exactas (nunca "latest"), lockfile, Dependabot, lint y formato |
| [Definition of Done](definition-of-done.md) | Checklist para dar una tarea por cerrada |
| [Onboarding](onboarding.md) | Pasos para una persona nueva |
