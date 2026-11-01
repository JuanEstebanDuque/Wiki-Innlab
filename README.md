# InnLab — Wiki de desarrollo

📍 **InnLab** (estás en la raíz)

> _[Descripción del área — la escribe el líder de InnLab: qué es InnLab, qué hace, a quién sirve, cómo se organiza el equipo.]_

**Esta wiki es la fuente de verdad** de cómo se trabaja en InnLab. Cada repo tiene un README corto que enlaza aquí.

---

## Lo mínimo que todo el que entra debe saber

Estas son las únicas reglas que aplican **a todo, siempre**. El detalle vive en las secciones de abajo.

1. **Todo el código vive en la organización de GitHub [`innlab2016`](https://github.com/innlab2016).**
2. **Nada entra a `main` sin Pull Request revisado.** Nunca se hace push directo a `main` ni a `dev`.
3. **Ningún secreto va al repositorio**: API keys, contraseñas, tokens y `.env` quedan fuera de git.
4. **Toda autenticación pasa por [InnLab Core](03-InnLab-Core/README.md).** Ningún producto tiene login propio.
5. **El código se escribe en inglés y la documentación en español.**
6. **El código nuevo entra con sus tests, en el mismo PR.**
7. **Si no está en la wiki, no es un estándar.** Los acuerdos verbales se documentan aquí.

**El estándar regula la forma de trabajar, no la tecnología.** Cada repo elige su gestor de paquetes, su framework de tests o su librería. Lo que se exige es cómo se nombra, se versiona, se prueba, se documenta y se revisa.

---

## Cómo leer esta wiki

```
InnLab (lo mínimo)
 ├── Estándares ........ reglas transversales, una card por tema
 ├── Testing ........... cómo se testea, sin atarse a un framework
 ├── InnLab Core ....... la plataforma base: cómo conectar un producto (SSO, API interna)
 ├── Stacks ............ cómo se aplican los estándares en cada tecnología que usamos hoy
 ├── Proyectos ......... cada producto y sus repos, con todo el detalle
 └── Referencia ........ mapa de servicios, plantillas, glosario, decisiones
```

- **Mientras más bajas, más detalle.** Cada nivel **hereda** del de arriba y puede especializar una regla, pero no contradecirla. Las excepciones se declaran explícitamente, con su motivo.
- **No se repite contenido:** una página de repo enlaza la regla de commits, no la copia.
- **Cada página empieza con 📍, que indica dónde estás en el árbol.**

---

## 🧱 Estándares

| Card | Qué resuelve |
| --- | --- |
| [Git y ramas](01-Estandares/git-y-ramas.md) | `main`/`dev`, ramas `feat/`, `fix/`… y cuándo se borran |
| [Commits](01-Estandares/commits.md) | Conventional Commits, con ejemplos buenos y malos |
| [Versionado y releases](01-Estandares/versionado-y-releases.md) | Qué versión poner (0.1.0, 1.0.0…) y cómo se hace un release |
| [Issues y Pull Requests](01-Estandares/issues-y-pull-requests.md) | Del issue al merge, plantilla de PR y checklist de review |
| [Estructura de carpetas](01-Estandares/estructura-de-carpetas.md) | Dónde va cada cosa en el back y el front, y cómo adoptarlo sin romper lo existente |
| [Documentación en el código](01-Estandares/documentacion-en-codigo.md) | Comentario de qué hace y para qué sirve encima de cada función pública |
| [Idioma y nomenclatura](01-Estandares/idioma-y-nomenclatura.md) | Qué va en inglés o en español; cómo nombrar archivos, clases, tablas y variables |
| [Seguridad, secretos y accesos](01-Estandares/seguridad-y-accesos.md) | `.env`, qué hacer si se filtra una key, dueños de cada cuenta |
| [Documentación](01-Estandares/documentacion.md) | Wiki como fuente de verdad, README corto, ADRs |
| [CI y calidad](01-Estandares/ci-y-calidad.md) | Checks de cada PR, cobertura mínima, SonarQube en backends y qué hacer sin protección de ramas |
| [Despliegue y CI/CD](01-Estandares/despliegue-y-ambientes.md) | PR → staging con validación manual → producción automática; rollback solo en producción |
| [Dependencias y herramientas](01-Estandares/dependencias-y-herramientas.md) | Versiones exactas, lockfile, Dependabot, lint y formato, sin imponer herramienta |
| [Definition of Done](01-Estandares/definition-of-done.md) | Cuándo una tarea está terminada de verdad |
| [Onboarding](01-Estandares/onboarding.md) | Tu primer día y tu primer PR |

## 🧪 Testing

[Principios y flujo](02-Testing/README.md) · [Qué probar en backend y frontend](02-Testing/que-probar-back-y-front.md) · [Qué y cómo testear](02-Testing/que-y-como-testear.md) · [Convenciones](02-Testing/convenciones.md)

## 🔐 InnLab Core

[Qué es y reglas de oro](03-InnLab-Core/README.md) · [Integrar un producto](03-InnLab-Core/integrar-un-producto.md) · [Autenticación y SSO](03-InnLab-Core/autenticacion-y-sso.md) · [API interna](03-InnLab-Core/api-interna.md) · [Estado y pendientes](03-InnLab-Core/estado-y-pendientes.md)

Repos: [innlab-core-api](03-InnLab-Core/innlab-core-api.md) · [innlab-core-user-space](03-InnLab-Core/innlab-core-user-space.md) · [product-integration-guide](03-InnLab-Core/product-integration-guide.md)

## ⚙️ Stacks

| Card | Usado en |
| --- | --- |
| [NestJS (backend)](04-Stacks/nestjs.md) | innlab-core-api, product-integration-guide, foresight-backend, indigo-backend |
| [React + Vite (frontend)](04-Stacks/react-vite.md) | innlab-core-user-space, foresight-user-space, foresight-landing, Growinn Frontend |
| [Vue + Vite (frontend)](04-Stacks/vue-vite.md) | indigo-user-space |
| [Express / Node (legado)](04-Stacks/express-node.md) | Growinn Backend |
| [Python / notebooks](04-Stacks/python-notebooks.md) | foresight-scrapping |

## 📦 Proyectos

| Proyecto | Qué es (resumen) | Repos |
| --- | --- | --- |
| [Foresight](05-Proyectos/foresight/README.md) | Vigilancia tecnológica e informes por vertical | 4 |
| [Índigo](05-Proyectos/indigo/README.md) | Diagnósticos organizacionales 1-3-9-45 con IA | 2 |
| [Growinn Index (Inndex)](05-Proyectos/growinn-index/README.md) | Encuestas de diagnóstico por áreas; será reemplazado por Índigo | 1 |

Catálogo completo: [05-Proyectos](05-Proyectos/README.md)

## 📚 Referencia

**[Pendientes por concretar](pendientes-por-concretar.md)** · [Mapa de servicios](06-Referencia/mapa-de-servicios.md) · [Plantillas](06-Referencia/plantillas.md) · [Glosario](06-Referencia/glosario.md)

---

## ¿Dónde busco…?

| Busco… | Voy a… |
| --- | --- |
| Cómo nombrar mi rama | [Git y ramas](01-Estandares/git-y-ramas.md) |
| Cómo escribir el mensaje de commit | [Commits](01-Estandares/commits.md) |
| Qué poner en el PR | [Issues y Pull Requests](01-Estandares/issues-y-pull-requests.md) |
| Dónde van los tests y cómo se llaman | [Testing → Convenciones](02-Testing/convenciones.md) |
| Qué pruebas y qué cobertura debe tener mi repo | [Qué probar](02-Testing/que-probar-back-y-front.md#pruebas-obligatorias-por-tipo-de-repo) y [CI y calidad](01-Estandares/ci-y-calidad.md) |
| Cómo armo el CI de un repo nuevo | [CI y calidad](01-Estandares/ci-y-calidad.md#qué-cambia-según-la-tecnología) |
| Qué hago antes de aprobar un PR | [Issues y Pull Requests → Checklist del revisor](01-Estandares/issues-y-pull-requests.md#checklist-del-revisor) |
| Cómo conecto mi producto al login de InnLab | [Integrar un producto](03-InnLab-Core/integrar-un-producto.md) |
| Qué endpoints de Core puedo usar | [API interna](03-InnLab-Core/api-interna.md) |
| Cómo crear un módulo en NestJS | [NestJS](04-Stacks/nestjs.md) |
| Cómo correr un repo en local | La página de ese repo |
| Quién me da acceso a una cuenta | [Seguridad y accesos](01-Estandares/seguridad-y-accesos.md) → "Dueños de cuentas" |
