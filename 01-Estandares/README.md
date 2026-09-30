# Estándares

📍 [InnLab](../README.md) › **Estándares**

Reglas que aplican a **todos los proyectos y repos** de InnLab. Cada card es independiente: puedes leer solo la que necesitas.

- **Regulan la forma de trabajar, no la tecnología.** Lo que depende del stack está en [Stacks](../04-Stacks/README.md).
- Lo marcado **(Propuesta)** o **(En definición)** todavía no está cerrado. Ver [Decisiones](../06-Referencia/decisiones-pendientes.md).
- Testing tiene su propia sección: [Testing](../02-Testing/README.md).

| Card | En una línea |
| --- | --- |
| [Git y ramas](git-y-ramas.md) | `main` + `dev`; ramas `<tipo>/<issue>-<descripcion>`; se borran al hacer merge |
| [Commits](commits.md) | `<tipo>(alcance): resumen en inglés, imperativo, ≤72 caracteres` |
| [Versionado y releases](versionado-y-releases.md) | SemVer por repo; cuándo es 0.1.0 o 1.0.0; release por PR `dev` → `main` con tag y CHANGELOG |
| [Issues y Pull Requests](issues-y-pull-requests.md) | Todo parte de un issue; PR con plantilla, 1 aprobación y squash merge |
| [Idioma y nomenclatura](idioma-y-nomenclatura.md) | Código en inglés, docs en español; tabla de convenciones de nombres |
| [Seguridad, secretos y accesos](seguridad-y-accesos.md) | Nada de secretos en git; cada cuenta tiene un dueño |
| [Documentación](documentacion.md) | La wiki es la fuente de verdad; README corto que enlaza aquí |
| [Despliegue y CI/CD](despliegue-y-ambientes.md) | Principios independientes de la plataforma, que está en definición |
| [Dependencias y herramientas](dependencias-y-herramientas.md) | Lockfile versionado, lint y formato, dependencias justificadas |
| [Definition of Done](definition-of-done.md) | Checklist para dar una tarea por cerrada |
| [Onboarding](onboarding.md) | Pasos para una persona nueva |
