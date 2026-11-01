# Pendientes por concretar

📍 [InnLab](README.md) › **Pendientes por concretar**

> **Resumen:** todo lo que falta para que el estándar quede cerrado y aplicado, en un solo lugar. Hay cinco bloques: decisiones por tomar, información por conseguir, pendientes de InnLab Core, acciones en cada repo y publicación de la wiki. Cuando un punto se cierra, se marca aquí y se actualiza la página correspondiente.
>
> Estado a 2026-09-27.

**Prioridad:** 🔴 bloquea otras cosas · 🟠 importante · 🟢 puede esperar

---

## 1. Decisiones por tomar

Hasta que se decidan, las reglas afectadas siguen marcadas como **(Propuesta)** o **(En definición)** en la wiki.

| # | Decisión | Prioridad | Opciones / propuesta actual | Qué desbloquea | Quién decide |
| --- | --- | --- | --- | --- | --- |
| 1.1 | **Plataforma de despliegue** | 🔴 | Servidores de la universidad (Docker o PaaS auto-hospedado), AWS o PaaS gestionado | Ambientes, URLs, CI/CD, despliegue de Índigo | _Equipo (en investigación)_ |
| 1.2 | **Base de datos estándar** | 🔴 | Servidores de la universidad. Falta definir el motor (hoy PostgreSQL en Core y Foresight, Supabase en Growinn e Índigo sin definir) | Migración de datos, BD de Índigo, `.env` de todos los backends | _Equipo (en investigación)_ |
| 1.3 | ~~Herramienta de CI/CD~~ ✅ **Decidido:** GitHub Actions con los runners de GitHub (sin servidor propio). Ver [CI y calidad](01-Estandares/ci-y-calidad.md) | — | — | — | _Equipo_ |
| 1.4 | ~~¿Existe ambiente de staging?~~ ✅ **Decidido:** sí; se despliega la rama de cada PR, quien revisa lo valida a mano y al fusionar pasa solo a producción (rollback solo en producción). Ver [Despliegue y CI/CD](01-Estandares/despliegue-y-ambientes.md). Falta montarlo (depende de 1.1) | 🟠 | — | — | Depende de 1.1 |
| 1.5 | **¿Se mantiene `.env.example`?** | 🟠 | Propuesta: sí (solo nombres de variables, sin valores) | Onboarding, plantilla de README | _Líder InnLab_ |
| 1.6 | Primera versión con el estándar | 🟠 | `0.1.0` en todos; Core y Foresight evalúan `1.0.0` | Primeros tags y CHANGELOG | _Líderes de proyecto_ |
| 1.7 | Número de issue obligatorio en el nombre de la rama | 🟢 | `feat/42-descripcion`, obligatorio en `feat` y `fix` | Trazabilidad issue → rama → PR | _Equipo_ |
| 1.8 | Estrategia de merge | 🟢 | Squash a `dev`, merge commit en los releases | Historial limpio, CHANGELOG | _Equipo_ |
| 1.9 | Términos de dominio en español en el código | 🟢 | Permitirlos si están en el glosario, o traducirlos (`diagnosticador`, `macro_categorias`) | Nomenclatura de Índigo | _Equipo de Índigo_ |
| 1.10 | Hooks de git (Husky + lint-staged + commitlint) | 🟢 | Adoptarlos en todos los repos | Formato de commits validado automáticamente | _Equipo_ |
| 1.11 | ~~Cobertura en CI~~ ✅ **Decidido:** mínimos de 70 % unitarias (back y front), 40 % integración y 15 % E2E (back). Ver [Convenciones](02-Testing/convenciones.md#cobertura) | — | — | — | _Equipo_ |
| 1.13 | **Plan de GitHub** para proteger ramas | 🟠 | El plan gratuito no protege ramas en repos privados: hoy es regla de equipo. Opciones: GitHub Team o repos públicos | Que el CI bloquee el merge por sí solo | _Líder InnLab_ |
| 1.14 | **Plan de SonarQube Cloud** | 🟢 | Gratuito: 50.000 líneas para toda la organización, 5 miembros y solo la rama principal y sus PR (por eso la rama principal en Sonar es `dev`, y solo se usa en backends). Con plan Team (o repos públicos) se pueden sumar los frontends | Sonar en más repos | _Líder InnLab_ |
| 1.12 | Versión mínima de Node y `.editorconfig` | 🟢 | Node 20 LTS o superior; UTF-8, LF, 2 espacios | Entornos iguales en todas las máquinas | _Equipo_ |

---

## 2. Información por conseguir (a nivel interno)

| # | Qué falta | Dónde se usa | A quién pedirlo |
| --- | --- | --- | --- |
| 2.1 | **Descripción del área InnLab** | [Raíz de la wiki](README.md), [Glosario](06-Referencia/glosario.md) | Líder de InnLab |
| 2.2 | Descripción de cada proyecto: InnLab Core, Foresight, Índigo, Growinn | Página de cada uno (hoy dice `_[Descripción…]_`) | Líder de cada proyecto |
| 2.3 | **Responsable** de cada proyecto | Ficha de cada proyecto ("por definir") | Líder de InnLab |
| 2.4 | **Dueño de cada cuenta o servicio** (GitHub, AWS, BD, APIs externas…) | [Seguridad y accesos](01-Estandares/seguridad-y-accesos.md) | Nivel interno |
| 2.5 | **Qué son Ucorp y diag-irl**: quién los lleva y dónde están sus repos | [Proyectos](05-Proyectos/README.md), [API interna](03-InnLab-Core/api-interna.md) | Equipo de Core |
| 2.6 | Cómo llegan los datos de `foresight-scrapping` a la BD de Foresight (¿manual, script?) | [Foresight](05-Proyectos/foresight/README.md) | Equipo de Foresight |
| 2.7 | ~~Índigo: qué BD usa y qué proveedor de LLM~~ ✅ PostgreSQL con migraciones; IA con Groq, Gemini y OpenRouter en cadena de respaldo | [indigo-backend](05-Proyectos/indigo/indigo-backend.md) | — |
| 2.8 | Plan y fechas del **reemplazo de Growinn por Índigo** | [Growinn](05-Proyectos/growinn-index/README.md), [Índigo](05-Proyectos/indigo/README.md) | Líder de InnLab / Índigo |
| 2.9 | ~~Slug y key interna de Índigo en Core~~ ✅ Índigo ya se autentica con Core | [Integrar un producto](03-InnLab-Core/integrar-un-producto.md) | — |

---

## 3. Pendientes de InnLab Core (plataforma)

Lo que Core todavía no ofrece y que los productos van a necesitar. Detalle en [Estado y pendientes de Core](03-InnLab-Core/estado-y-pendientes.md).

| # | Pendiente | Prioridad | Impacto si no se hace |
| --- | --- | --- | --- |
| 3.1 | **Roles por empresa (`companyRole`)** disponibles en el backend del producto: interceptor reutilizable en `product-integration-guide` | 🟠 | Cada producto lo implementa a su manera, o no puede usar `@Roles()` |
| 3.2 | **Asignación de roles** real (`POST /internal/roles/assign` hoy es un stub) | 🟠 | Los productos no pueden gestionar roles |
| 3.3 | **Catálogo de productos** (`GET /hub/products`), en vez de tenerlos hardcodeados en el frontend | 🟠 | Cada producto nuevo requiere un cambio de código en el hub |
| 3.4 | Registro de productos y keys sin intervención manual | 🟢 | Depende del equipo de Core para cada integración |
| 3.5 | Feature flags administrables (hoy solo lectura) | 🟢 | Se cargan a mano en la BD |
| 3.6 | Módulo de workspaces activo | 🟢 | Las entidades existen, pero no se pueden usar |
| 3.7 | Panel de administración (miembros, invitaciones) | 🟢 | Todo se gestiona manualmente |

---

## 4. Acciones en cada repo para alinearse al estándar

### Para todos los repos

- [ ] README corto con enlace a su página en Notion ([plantilla](06-Referencia/plantillas.md))
- [ ] `.gitignore` mínimo: `.env` y archivos de asistentes de IA ([Seguridad](01-Estandares/seguridad-y-accesos.md))
- [ ] Ramas `main` y `dev`, protegidas cuando el plan lo permita (1.13); mientras tanto, la regla de equipo de [CI y calidad](01-Estandares/ci-y-calidad.md)
- [ ] CI con GitHub Actions según [CI y calidad](01-Estandares/ci-y-calidad.md): lint sin advertencias, build, pruebas con cobertura mínima, auditoría y simulacro de promoción (`indigo-backend` e `indigo-user-space` sirven de plantilla)
- [ ] Versiones fijadas y `dependabot.yml` ([Dependencias](01-Estandares/dependencias-y-herramientas.md#versiones-fijadas))
- [ ] Backends: SonarQube Cloud activado con el secreto `SONAR_TOKEN`
- [ ] Activar *Automatically delete head branches* y limpiar las ramas fuera del estándar
- [ ] `.github/pull_request_template.md` y plantillas de issue
- [ ] Scripts estándar: `dev`, `build`, `lint`, `format`, `test`
- [ ] `.env.example` al día (si se confirma 1.5)
- [ ] Código nuevo en la [estructura de carpetas](01-Estandares/estructura-de-carpetas.md) estándar; mover lo existente en PRs `refactor:` aparte
- [ ] Comentario de qué hace y para qué sirve en funciones, métodos y componentes públicos ([Documentación en el código](01-Estandares/documentacion-en-codigo.md)), al tocarlos

### Por repo

| Repo | Qué falta (lo más importante primero) | Prioridad |
| --- | --- | --- |
| [foresight-backend](05-Proyectos/foresight/foresight-backend.md) | **Revisar `getReportPreview`: endpoint público sin verificación de usuario** · sacar `.claude/` del repo · README dice Keycloak (es Cognito) · cero tests · reactivar reglas de ESLint · limpiar ramas | 🔴 |
| [foresight-scrapping](05-Proyectos/foresight/foresight-scrapping.md) | **Rotar la key de SerpAPI expuesta** (la rota el dueño) y sacar credenciales y correos personales de los notebooks · README, `.gitignore`, `requirements.txt` · crear `dev` · renombrar notebooks | 🔴 |
| [foresight-landing](05-Proyectos/foresight/foresight-landing.md) | **Declarar `react-router-dom` en `package.json`** (un `npm install` limpio falla) · sincronizar `main` con `dev` · borrar stubs vacíos y carpetas duplicadas | 🔴 |
| [indigo-backend](05-Proyectos/indigo/indigo-backend.md) | Alineado con el estándar (auth con Core, migraciones, ESLint, CI con SonarQube, Swagger). Queda: decidir el prefijo `/api` | 🟢 |
| [indigo-user-space](05-Proyectos/indigo/indigo-user-space.md) | Alineado con el estándar (SSO de Core, CI con Playwright). Queda: **conectar el portal empresa al backend** (hoy usa datos de ejemplo) | 🟠 |
| [innlab-core-api](03-InnLab-Core/innlab-core-api.md) | README real · `.env.example` · CORS desde variable de entorno · `logging: true` solo en desarrollo · tests de guards y SSO · Swagger · limpiar ramas | 🟠 |
| [innlab-core-user-space](03-InnLab-Core/innlab-core-user-space.md) | Crear `dev` · README real · `.env.example` · URLs de productos a variables de entorno · tests | 🟠 |
| [product-integration-guide](03-InnLab-Core/product-integration-guide.md) | Crear `dev` · `GUIDE.md` que enlace a la wiki · tests de `/health` y del guard | 🟢 |
| [foresight-user-space](05-Proyectos/foresight/foresight-user-space.md) | `HeroCards.tsx` huérfano · carpetas duplicadas · nombres de archivo en inglés y PascalCase · tests · limpiar 11+ ramas | 🟢 |
| [-Growinn-index-test](05-Proyectos/growinn-index/growinn-index-test.md) | **Ninguna**: se mantiene tal cual hasta que Índigo lo reemplace | — |

---

## 5. Publicar y adoptar la wiki

| # | Paso | Estado |
| --- | --- | --- |
| 5.1 | Revisar y aprobar esta primera versión de la wiki | ⏳ |
| 5.2 | Completar las descripciones y los responsables (bloque 2) | ⏳ |
| 5.3 | Importar a Notion (carpeta `InnLab-Wiki` → zip → *Import → Markdown & CSV*) | ⏳ |
| 5.4 | Con las URLs de Notion, poner el enlace en el README de cada repo | ⏳ (depende de 5.3) |
| 5.5 | **Socializar con los equipos**: hoy no conocen las reglas (por ejemplo, `feat/`) | ⏳ |
| 5.6 | Reemplazar los documentos anteriores por un enlace a la wiki: `InnLab Project.md`, `Foresight/PROJECT_STANDARDS.md`, los `CONTRIBUTING.md` de Foresight y `GitHubWorkflowGuide.md` de Growinn | ⏳ |
| 5.7 | Definir quién mantiene cada sección de la wiki (Core: equipo de Core; cada proyecto: su líder técnico) | ⏳ |
| 5.8 | Cuando se cierre 1.1: escribir el ADR, reemplazar el simulacro de promoción por el despliegue real y actualizar [Despliegue y CI/CD](01-Estandares/despliegue-y-ambientes.md) | ⏳ |
