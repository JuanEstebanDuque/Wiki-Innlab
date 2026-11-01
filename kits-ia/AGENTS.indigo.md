# AGENTS.md — Índigo (InnLab)

Eres un asistente de código trabajando en **Índigo**, la plataforma de diagnósticos organizacionales de InnLab. Un profesor define un tema, el sistema busca literatura (Tavily y OpenAlex), el profesor elige las fuentes y un LLM genera en cadena la estructura **1-3-9-45**: 1 diagnóstico → 3 macro categorías → 9 categorías → 45 ítems Likert-6. Índigo reemplazará a Growinn Index. Sigue estas reglas en todo cambio.

## Repos del proyecto
- `indigo-backend`: NestJS 12 + TypeScript + Zod + Vitest + `pg`. Gestor **pnpm**. Es el repo modelo en testing.
- `indigo-user-space`: Vue 3 + Vite + TypeScript (`vue-tsc`). npm. Vue es una decisión tomada: no proponer migrar a React.

## Autenticación (crítico)
- La autenticación va **siempre por InnLab Core** (pendiente de integrar). Backend: guard JWT global y `InnlabApiService` copiados de `product-integration-guide`, `@Public()` en `/health`. Frontend: una ruta del router recibe `?code=`, canjea el código en Core y guarda el `accessToken`.
- Nunca construir login, usuarios ni roles propios. Nunca JOIN con la BD de Core.

## indigo-backend
- Estructura de referencia: `src/config/`, `src/common/` (decorators, filters, guards, interceptors, middleware) y el módulo `src/diagnosticador/` con `dto/`, `entities/`, `interfaces/`, `search/`, `generate/` y `database/`.
- Pipeline lineal (sin agentes): búsqueda en paralelo → enriquecimiento de papers → el profesor revisa fuentes → macros → categorías → ítems → persistencia.
- **Toda salida del LLM se valida con un esquema Zod** antes de pasar al siguiente paso. Si una llamada falla, se reintenta solo esa.
- Prompts y modelos centralizados en `generate/` y `claudeClient.ts`: cambiar de modelo es configuración, no código.
- Documentar el costo por llamada de servicios pagos (OpenAlex Fulltext).
- Cambios de esquema solo por migraciones versionadas (no SQL en el README).
- Tests con Vitest: un `*.spec.ts` junto a cada archivo de `search/`, `generate/` y `database/`. Mínimos en el CI: unitarias 70 %, integración con Postgres real 40 % (`pnpm test:int`), E2E por HTTP 15 % (`pnpm test:e2e`), y SonarQube Cloud en verde.
- Sus workflows (`ci.yml`, `smoke.yml`, `promocion.yml`) son la plantilla de los demás backends: no los rompas.
- Lint: se migra de oxlint a ESLint; `no-explicit-any` debe quedar activo.
- Términos de dominio en español (`diagnosticador`, `macro_categorias`) están en discusión: no renombrar sin acuerdo del equipo, y no crear términos nuevos en español.

## indigo-user-space
- `<script setup lang="ts">` en todos los componentes; nada de Options API.
- Estructura por feature: `src/modules/<feature>/` (components, views, composables, `index.ts`), `components/ui/` (prefijo `Base`), `composables/`, `services/` (único lugar con HTTP), `stores/` (Pinia solo si hace falta), `config.ts` (único lugar que lee `VITE_*`).
- Componentes en PascalCase y con más de una palabra; props y emits tipados.
- Tests con Vitest + Vue Test Utils (≥ 70 % de líneas): composables, `services/` y recepción del SSO. E2E con Playwright solo para flujos críticos, con la API simulada (`page.route`).

## Reglas generales de InnLab
- Código, commits y ramas en inglés; comentarios y docs en español. Toda función/componente público con comentario JSDoc: qué hace y para qué sirve.
- Ramas `<tipo>/<issue>-<descripcion>` desde `dev`; commits Conventional Commits; PR hacia `dev` con `Fixes #n`.
- Toda lógica nueva con su test en el mismo cambio. Mocks para Core, LLM, Tavily y OpenAlex.
- Errores tipados, sin `catch` vacíos, sin loguear claves ni datos personales. `GET /health` público en el backend.

## Nunca
- Commitear `.env`, API keys (OpenRouter, Anthropic, Tavily, OpenAlex) ni datos personales.
- Commitear `AGENTS.md`, `CLAUDE.md`, `.claude/` ni `docs/superpowers/` u otros specs generados.

## Antes de dar un cambio por terminado
- Backend: `pnpm lint`, `pnpm test:cov` y `pnpm build` pasan. Frontend: `npm run lint`, `npm run test:cov`, `npm run build`. Lint con cero advertencias.
- Versión subida en `package.json` y entrada en `CHANGELOG.md`. Dependencias con versión exacta.
- README y `.env.example` actualizados si cambió la configuración.
