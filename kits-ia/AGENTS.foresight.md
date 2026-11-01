# AGENTS.md — Foresight (InnLab)

Eres un asistente de código trabajando en **Foresight**, el producto de vigilancia tecnológica de InnLab: genera informes y radares por vertical (fintech, agrotech, biotech…) a partir de papers, patentes y noticias, con planes de suscripción y un servicio a la medida (custom made). Sigue estas reglas en todo cambio.

## Repos del proyecto
- `foresight-backend`: NestJS 11 + TypeORM + PostgreSQL + AWS S3 + Swagger. npm. Puerto 3002. Jest.
- `foresight-user-space`: app del cliente. React 19 + Vite + Tailwind, **JavaScript** (no TypeScript). npm. Puerto 5174.
- `foresight-landing`: sitio público. React 19 + Vite + Tailwind, JavaScript, estructura por tipo (`pages/`, `components/`, `constants/`).
- `foresight-scrapping`: notebooks de Python (Colab) que recolectan papers (Crossref, Unpaywall, OpenAlex), patentes (PatentsView) y noticias (SerpAPI).

## Autenticación (crítico)
- El usuario entra desde el hub de **InnLab Core** por SSO: el front recibe `?code=`, lo canjea en `GET {VITE_CORE_API_URL}/auth/sso/exchange` y guarda el `accessToken` (no el `token`/id_token).
- El backend valida el JWT localmente contra el JWKS de Cognito (`RS256`, `token_use === 'access'`) con guard global; rutas públicas con `@Public()`.
- Empresa, rol y miembros se piden a Core con `InnlabApiService` (`/internal/*`, `CORE_INTERNAL_KEY`). Nunca crear login, usuarios ni roles propios. Nunca JOIN con la BD de Core.

## foresight-backend
- Módulos en `src/modules/<dominio>/` (assets, custom-made, dashboard, me, reports, s3, subscriptions, verticals) con `module`, `controller`, `service`, `dto/`, `entities/`. Controller delgado; lógica en el service.
- Configuración nueva siempre en `src/config/namespaces/` validada con Joi.
- Migraciones solo con la CLI de TypeORM; `synchronize: false`. Migraciones que solo agregan.
- Todo endpoint nuevo lleva `@ApiOperation` y `@ApiResponse` (Swagger) y DTOs con `class-validator`.
- Prefijo `/api`, rutas en plural y kebab-case. Mantener `GET /health` público.
- Ojo: `reports.service.ts → getReportPreview` es público sin verificación de usuario; no copiar ese patrón.
- Variables: `FORESIGHT_DB_*`, `AWS_*`, `AWS_S3_*`, `COGNITO_ISSUER`, `COGNITO_JWKS_URI`, `INNLAB_API_BASE_URL`, `INNLAB_API_TIMEOUT`, `CORE_INTERNAL_KEY`, `CORS_ORIGINS`.
- Tests: `*.spec.ts` junto al archivo (hoy hay cero; prioridad `reports`, `subscriptions` y el cliente de Core). e2e en `test/*.e2e-spec.ts`.

## foresight-user-space
- Se mantiene en **JavaScript**: no crear archivos `.ts`/`.tsx`.
- Estructura por feature: `src/app/` (layouts, providers, router), `src/modules/<feature>/` (components, pages, mocks, `index.js`), `src/services/` (auth, foresight, innlab), `src/components/ui/`.
- Llamadas a Foresight solo por `services/foresight`; a Core por `services/innlab`. Nunca `fetch`/`axios` dentro de un componente.
- Componentes en PascalCase con el archivo del mismo nombre; nombres en inglés.
- Los mocks de `modules/<feature>/mocks` no se usan en producción.
- Variables: `VITE_FORESIGHT_API_URL`, `VITE_CORE_API_URL`, `VITE_CORE_URL` (sin secretos).

## foresight-landing
- Página nueva: crear en `pages/`, registrar la ruta en `AppRouter.jsx`, datos estáticos en `constants/`.
- Carpeta y archivo del componente coinciden en mayúsculas/minúsculas.

## foresight-scrapping
- Credenciales con `os.environ[...]` o `google.colab.userdata.get(...)`. **Nunca** una key literal en una celda.
- Correos de contacto de APIs: el del proyecto, no uno personal.
- Funciones repetidas entre notebooks → `src/`; tests con pytest en `tests/`.
- Limpiar las salidas de las celdas antes de commit.

## Reglas generales de InnLab
- Código, commits y ramas en inglés; comentarios y docs en español. Toda función/componente público con comentario JSDoc/docstring: qué hace y para qué sirve.
- Ramas `<tipo>/<issue>-<descripcion>` desde `dev`; commits Conventional Commits (`feat(reports): add pdf export`); PR hacia `dev` con `Fixes #n`.
- Toda lógica nueva con su test en el mismo cambio. Mocks para Core, Cognito, S3 y APIs externas. Mínimos: 70 % unitarias (back y front), 40 % integración y 15 % E2E en backend; mientras el repo no llegue, la cobertura no puede bajar.
- Cada PR sube `"version"` en `package.json` (SemVer) y agrega su entrada en `CHANGELOG.md`.
- Lint con cero advertencias; versiones exactas de dependencias (sin `^` ni `~`).
- Errores tipados, sin `catch` vacíos, sin `console.log` de depuración, sin loguear tokens ni datos personales.

## Nunca
- Commitear `.env`, keys, tokens o datos personales.
- Commitear `AGENTS.md`, `CLAUDE.md`, `.claude/` ni specs generados.
- Copiar `.env` o credenciales de Foresight a otro proyecto.

## Antes de dar un cambio por terminado
- `npm run lint`, `npm run test:cov` y `npm run build` pasan.
- Versión subida y entrada en `CHANGELOG.md`.
- README y `.env.example` actualizados si cambió la configuración.
