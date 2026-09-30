# innlab-core-api

📍 [InnLab](../README.md) › [InnLab Core](README.md) › **innlab-core-api**

> **Resumen:** API NestJS de InnLab Core. Valida los tokens de Cognito, maneja el SSO hacia los productos, el onboarding de empresas y la API interna que usan los backends de los productos.

## Ficha

| | |
| --- | --- |
| Repo | [innlab2016/innlab-core-api](https://github.com/innlab2016/innlab-core-api) |
| Stack | [NestJS](../04-Stacks/nestjs.md) 11 · TypeORM · PostgreSQL · Jest |
| Ramas | `main`, `dev` (+ `feature/internal-staff-accounts`, `flowCheck` por limpiar) |
| Puerto local | 3001 (`/api`) |

## Cómo correrlo

```bash
npm install
cp .env.example .env   # ⚠️ hoy no existe .env.example, ver deuda técnica
npm run start:dev
```

## Estructura

```
src/modules/
  auth/         → login con Cognito, SSO (initiate/exchange), logout, introspect, CognitoGuard
  onboarding/   → alta de empresa y primer acceso
  internal/     → API para otros backends (InternalGuard + key por servicio)
  companies/    → entidades y DTOs
  features/ products/ users/ workspaces/ → solo entidades
```

## Reglas específicas de este repo

- **`auth/` e `internal/` son el contrato del ecosistema.** Cualquier cambio sigue las reglas de [API interna → Cambios en este contrato](api-interna.md).
- Los endpoints de `/api/auth/*` llevan headers `no-store` (ya implementado en `main.ts`). No se quitan.
- Una key nueva de producto se agrega en `SERVICE_KEY_ENV` (`internal.guard.ts`) y en las variables de entorno. Nunca se pone el valor en el código.

## Variables de entorno

`PORT`, `DB_HOST`, `DB_PORT`, `DB_USERNAME`, `DB_PASSWORD`, `DB_NAME`, `DB_SSL`, `AWS_REGION`, `COGNITO_REGION`, `COGNITO_ISSUER`, `COGNITO_JWKS_URI`, `INTERNAL_KEY_FORESIGHT`, `INTERNAL_KEY_INNDEX`, `INTERNAL_KEY_UCORP`, `INTERNAL_KEY_DIAG_IRL`.

## Testing

Jest configurado, pero solo existe el test de la plantilla. Prioridad: `CognitoGuard`, `InternalGuard` y el flujo SSO (ver [Testing](../02-Testing/README.md)).

## Deuda técnica / plan de remediación

1. Reemplazar el README de la plantilla de NestJS por un README corto que enlace a esta sección de la wiki.
2. Crear `.env.example`.
3. Mover los orígenes CORS hardcodeados de `main.ts` a una variable de entorno.
4. Quitar `logging: true` de TypeORM fuera de desarrollo.
5. Tests de guards y del SSO.
6. Swagger.
7. Limpiar las ramas `flowCheck` y `feature/internal-staff-accounts`.
8. Los pendientes de funcionalidad están en [Estado y pendientes](estado-y-pendientes.md).
