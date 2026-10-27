# AGENTS.md — InnLab (reglas generales)

Eres un asistente de código trabajando en un repo de InnLab (organización de GitHub `innlab2016`). Sigue estas reglas en todo cambio. Si una regla del proyecto específico choca con esta, gana la del proyecto. Wiki completa: Notion → "InnLab — Wiki de desarrollo".

## Ecosistema
- Todos los productos se autentican con **InnLab Core** (AWS Cognito + SSO). Nunca crees login, tabla de usuarios, User Pool ni roles propios.
- Datos de empresa, rol y miembros se piden a Core por la API interna (`/internal/*`, header `x-internal-key`). Nunca JOIN entre la BD de un producto y la de Core; los IDs de Core se guardan como texto.
- La key interna (`CORE_INTERNAL_KEY`) vive solo en el backend. Nunca en un frontend ni en variables `VITE_*`.
- Un backend nuevo arranca desde el repo `product-integration-guide`.

## Idioma y nombres
- Código, archivos, commits y ramas en **inglés**. Comentarios, README y textos de documentación en **español**.
- camelCase (variables/funciones), PascalCase (clases, componentes), UPPER_SNAKE_CASE (constantes y env vars), kebab-case (carpetas y archivos TS de backend: `reports.service.ts`), snake_case (tablas y columnas, tablas en plural; Python).
- Rutas REST: plural, kebab-case, prefijo `/api`, sin versión.
- No mezcles idiomas en un identificador (`pagosAmedida` ❌ → `customPayments` ✅).

## Estructura
- Backend (NestJS): `src/config/`, `src/common/`, `src/auth/` y un módulo por dominio en `src/<modulo>/` con `module`, `controller`, `service` y solo las carpetas que use (`dto/`, `entities/`, `interfaces/`).
- Frontend (React/Vue): `src/modules/<feature>/` (components, pages|views, hooks|composables, `index.ts`), `components/ui/`, `services/` (único lugar con llamadas HTTP), `config.ts` (único lugar que lee `VITE_*`).
- Un módulo no importa archivos internos de otro, solo su API pública.
- Nunca muevas archivos y cambies su lógica en el mismo cambio.

## Reglas de código
- Controller delgado; la lógica va en el service. Inyección por constructor.
- Toda entrada se valida (DTO con class-validator o Zod). Nada de `any` en código nuevo.
- Respuestas de APIs externas o de un LLM se validan con un esquema antes de usarse.
- Configuración validada al arrancar. Nada hardcodeado por ambiente (URLs, CORS, puertos): todo de variables de entorno.
- Base de datos solo con migraciones versionadas; las migraciones solo agregan (borrar columnas va en un release posterior).
- Errores con excepciones tipadas y filtro global. Nunca `catch` vacío. Logs con el Logger del framework, sin `console.log`; nunca loguear tokens, contraseñas ni datos personales.
- Todo backend expone `GET /health` público (lo usa el despliegue blue-green).
- Toda función, método o componente público lleva encima un comentario JSDoc/docstring en español: qué hace y para qué sirve.

## Tests
- Toda lógica nueva o corregida entra con su test en el mismo cambio. Lo puramente visual no lleva test automático.
- Tests junto al archivo: `nombre.spec.ts` (backend), `Componente.test.tsx|ts` (frontend), `tests/test_<modulo>.py` (Python).
- Estructura Preparar → Ejecutar → Verificar; nombre que describe el comportamiento; caso feliz + al menos un caso de error.
- Todo lo externo (Core, Cognito, S3, APIs de IA, correo) se mockea. Datos ficticios.
- Cobertura mínima de líneas (el CI falla por debajo): unitarias 70 % (back y front); en backend además integración con BD real 40 % y E2E por HTTP 15 %. En frontend, E2E con Playwright solo para flujos críticos.
- Scripts estándar: `test`, `test:cov`, `test:int`, `test:e2e`. No escribas pruebas vacías para inflar la cobertura.

## Git
- Ramas: `<tipo>/<issue>-<descripcion-kebab>` desde `dev` (ej. `feat/42-pdf-export`). Tipos: feat, fix, hotfix (desde main), refactor, docs, chore.
- Commits Conventional Commits en inglés e imperativo, ≤72 caracteres: `feat(reports): add pdf export`. Un commit = un cambio lógico. Cambio que rompe: `feat!:` + `BREAKING CHANGE:`.
- PR hacia `dev` con la plantilla del repo (incluida "Cómo validarlo en staging"); `Fixes #n` en la descripción.
- Cada PR es un release: sube `"version"` en `package.json` según SemVer (fix → PATCH, feat → MINOR, breaking → MAJOR) y agrega la entrada en `CHANGELOG.md`. El tag y el paso a producción son automáticos.
- El CI debe pasar: instalación con lockfile congelado, lint con cero advertencias, build, pruebas con cobertura mínima, auditoría de dependencias de producción y, en backends, SonarQube Cloud.
- Quien revisa valida en staging y fusiona; nadie fusiona su propio PR.

## Dependencias
- Versiones exactas en el manifiesto (sin `^` ni `~`), lockfile versionado, runtime fijado en `.nvmrc`. Nunca "latest" (tampoco en imágenes de Docker ni en GitHub Actions, que van por SHA).

## Nunca
- Commitear `.env`, keys, tokens, certificados o datos personales.
- Commitear archivos de asistentes de IA: `AGENTS.md`, `CLAUDE.md`, `.claude/`, `.cursorrules`, specs o planes generados.
- Push directo a `main` o `dev`.
- Agregar una dependencia sin justificarla; nunca paquetes que imitan módulos nativos (`fs`, `path`, `https`).

## Antes de dar un cambio por terminado
- Correr `lint` (sin advertencias), `test:cov` y `build` del repo y que pasen.
- Versión subida y entrada en `CHANGELOG.md`.
- Si cambió cómo se instala, se corre o se configura: actualizar README y `.env.example`.
