# indigo-backend

📍 [InnLab](../../README.md) › [Proyectos](../README.md) › [Índigo](README.md) › **indigo-backend**

> **Resumen:** API NestJS del diagnosticador de Índigo. Busca literatura, genera la estructura 1-3-9-45 con un LLM y la guarda. **Es el repo modelo en testing**: cada archivo del pipeline tiene su `.spec.ts`.

## Ficha

| | |
| --- | --- |
| Repo | [innlab2016/indigo-backend](https://github.com/innlab2016/indigo-backend) |
| Stack | [NestJS](../../04-Stacks/nestjs.md) 12 · TypeScript 6 · Zod · Vitest · `pg` |
| Gestor de paquetes | pnpm |
| Lint | oxlint → **se migra a ESLint**, alineado con el resto de repos |
| Ramas | `master`, `dev` + ramas `feat/*`. **`master` se renombra a `main`** |
| Despliegue | Aún no; próximamente con el nuevo estándar |

## Cómo correrlo

```bash
pnpm install
pnpm approve-builds --all
cp .env.example .env     # ⚠️ hoy no existe .env.example
pnpm start:dev
pnpm cli:search "<tema>" # prueba el paso de búsqueda desde la terminal
```

El esquema de BD está hoy como SQL en el README. Ver deuda técnica.

## Estructura

```
src/diagnosticador/
  search/     → searchTavily, searchOpenAlex, fetchFulltext, fetchWebContent, rankSources
  generate/   → claudeClient (OpenRouter + fallback), generateMacros, generateCategorias, generateItems
  save/       → saveDiagnostic
  cli/        → searchCli (prueba del pipeline desde la terminal)
  types.ts
  diagnosticador.{module,controller,service}.ts
postman/                 → colección de endpoints
```

## Endpoints

- `POST /diagnosticador/search` → busca y rankea las fuentes (stateless).
- `POST /diagnosticador/generate` → recibe `{ topic, sources }` y genera 1-3-9-45.
- Guardado del diagnóstico en BD.

Colección completa en `postman/Indigo.postman_collection.json`.

## Variables de entorno

Según el código: `PORT`, `ANTHROPIC_API_KEY`, `TAVILY_API_KEY`, `OPENALEX_API_KEY`. Según el README: `DATABASE_URL` y `OPENROUTER_API_KEY`. **No coinciden**, ver deuda técnica.

## Testing

Vitest, con un `*.spec.ts` junto a cada archivo de `search/`, `generate/` y `save/`. **Es el ejemplo a seguir** para los demás backends.

```bash
pnpm test         # unitarios
pnpm test:cov     # cobertura
pnpm test:e2e
```

## Deuda técnica / plan de remediación

1. **Integrar la autenticación con InnLab Core** partiendo de [product-integration-guide](../../03-InnLab-Core/product-integration-guide.md): guard JWT global, `@Public()` en `/health` y cliente de la API interna.
2. **Unificar la documentación:** el README y el código no coinciden en el proveedor de LLM (Anthropic directo u OpenRouter), en la BD (Postgres local o Supabase) ni en las variables de entorno. El README corto y la wiki son la referencia.
3. **Sacar del repo los archivos de Claude:** `.claude/settings.local.json` y `docs/superpowers/specs/` están versionados. `claude.md` no está subido y así debe seguir. Agregarlos al `.gitignore` (ver [Seguridad](../../01-Estandares/seguridad-y-accesos.md)).
4. Si algo del spec del wizard sirve al equipo, reescribirlo en la wiki (página de Índigo) o como ADR.
5. Crear `.env.example`.
6. Renombrar `master` a `main`.
7. Pasar el SQL del README a migraciones versionadas.
8. Asegurar que el archivo de secretos se llame solo `.env`.
9. Commits: los últimos juntan varios cambios en uno (ver [Commits](../../01-Estandares/commits.md)).
10. Migrar de oxlint a ESLint y activar `no-explicit-any` (hoy está en `off`).
11. Decidir si los identificadores de dominio siguen en español.
