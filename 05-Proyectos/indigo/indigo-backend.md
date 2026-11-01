# indigo-backend

📍 [InnLab](../../README.md) › [Proyectos](../README.md) › [Índigo](README.md) › **indigo-backend**

> **Resumen:** API NestJS de Índigo: el diagnosticador (busca literatura y genera la estructura 1-3-9-45 con IA), las aplicaciones a empresas con sus resultados, el portal comercial y el portal empresa. Autenticación con InnLab Core, PostgreSQL con migraciones y **CI completo con SonarQube Cloud**: es el **repo de referencia de backend** para el estándar de [CI y calidad](../../01-Estandares/ci-y-calidad.md).

## Ficha

| | |
| --- | --- |
| Repo | [innlab2016/indigo-backend](https://github.com/innlab2016/indigo-backend) |
| Stack | [NestJS](../../04-Stacks/nestjs.md) 12 · TypeScript 6 · Zod · Vitest · `pg` · pg-boss · Sentry |
| Versiones | pnpm 11.3.0 (`packageManager`) · Node 24.13.1 (`.nvmrc`) · dependencias exactas |
| Lint | ESLint sin advertencias (`--max-warnings 0`) · Prettier |
| Ramas | `main` y `dev` (ver [Git y ramas](../../01-Estandares/git-y-ramas.md)) |
| Autenticación | InnLab Core: JWT de Cognito validado en un guard global, `@Public()` en las rutas abiertas y roles por `@Roles()` |
| Documentación de la API | **Swagger** en `/docs` (OpenAPI en `/docs/json`) |
| Despliegue | Aún no: plataforma en definición. Hoy corre el [simulacro de promoción](../../01-Estandares/despliegue-y-ambientes.md) |

## Cómo correrlo

```bash
pnpm install
cp .env.example .env    # los valores los da el responsable del proyecto
docker compose up -d     # Postgres 16 local
pnpm db:migrar           # aplica las migraciones de db/migrations
pnpm start:dev           # http://localhost:3000 · Swagger en /docs
```

Con `AUTH_DEV_USUARIO_ID` en el `.env` (solo con `NODE_ENV=development`) el backend responde como ese usuario sin pasar por InnLab Core.

## Módulos

| Módulo | Qué hace |
| --- | --- |
| `diagnosticador/` | Búsqueda (OpenAlex, fuentes web), generación 1-3-9-45 en segundo plano (pg-boss), borradores, revisión de preguntas y publicación |
| `aplicaciones/` | Aplicaciones de un diagnóstico a empresas, cobertura, resultados y análisis comparativo |
| `comercial/` | Oportunidades, invitaciones a empresas (link con token) y aprobación de solicitudes |
| `empresa/` | Portal empresa: catálogo, solicitudes, aplicaciones, resultados, equipo y áreas, plan y método de pago, reportes |
| `notificaciones/` · `protocolo/` · `usuarios/` · `admin/` | Notificaciones, protocolo académico, usuario actual y vista del admin |
| `common/` | Auth con Core, base de datos, filtros, guards, límite de tasa, resiliencia (circuit breaker, reintentos, deadlines), trabajos, correo, observabilidad, Swagger |

La lista completa de endpoints está en **Swagger** (`/docs`), con quién puede usar cada uno; el detalle de flujos, en `docs/endpoints.md`, y la colección de Postman, en `postman/`.

## Variables de entorno

Todas están en `.env.example` y se validan al arrancar (en producción, si falta una, la app no levanta): base de datos, CORS, InnLab Core (Cognito, API interna y hub), IA (Groq, Gemini y OpenRouter en cadena de respaldo; basta una key), OpenAlex, SMTP, Swagger (`SWAGGER_HABILITADO`) y proxy.

## Base de datos

PostgreSQL con **migraciones versionadas** en `db/migrations` (`001`–`015`), aplicadas con `pnpm db:migrar`, y un seed en `db/seed`. Detalle en `db/README.md`.

## Testing y CI

| Nivel | Dónde | Mínimo de líneas |
| --- | --- | --- |
| Unitarias | `src/**/*.spec.ts` | 70 % |
| Integración (Postgres real) | `test/integration/*.int-spec.ts` | 40 % |
| E2E por HTTP | `test/e2e/*.e2e-spec.ts` | 15 % |

```bash
pnpm test        # unitarias
pnpm test:todo   # las tres suites con cobertura
pnpm lint
```

El CI (`.github/workflows/ci.yml`) corre en cada PR:
1. Auditoría de dependencias de producción.
2. Lint, build y unitarias.
3. Integración y E2E contra Postgres.
4. **SonarQube Cloud** con la cobertura de las tres suites; en SonarQube la rama principal del proyecto es `dev`.

También tiene `smoke.yml` (revisión de un ambiente desplegado) y `promocion.yml` (simulacro `dev` → `main`). Dependabot abre PRs semanales contra `dev`; para que el check de Sonar pase en esos PRs, `SONAR_TOKEN` también va en los secretos de Dependabot.

## Pendientes

1. **Prefijo `/api`:** la [regla de NestJS](../../04-Stacks/nestjs.md) lo pide y hoy las rutas no lo tienen. Cambiarlo afecta al front y a los enlaces de invitación ya enviados: se decide con el equipo.
2. **Pasarela de pago real:** el plan y el método de pago del portal empresa son simulados (solo se guardan la marca y los 4 últimos dígitos).
3. Decidir si los identificadores de dominio siguen en español (ver [Pendientes por concretar](../../pendientes-por-concretar.md)).
