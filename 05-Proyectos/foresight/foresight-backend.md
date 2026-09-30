# foresight-backend

📍 [InnLab](../../README.md) › [Proyectos](../README.md) › [Foresight](README.md) › **foresight-backend**

> **Resumen:** API NestJS de Foresight, con informes, verticales, suscripciones, assets, dashboard y S3. Valida la sesión con Cognito y consulta datos de empresa y usuario en InnLab Core por la [API interna](../../03-InnLab-Core/api-interna.md). Es la integración de referencia con Core.

## Ficha

| | |
| --- | --- |
| Repo | [innlab2016/foresight-backend](https://github.com/innlab2016/foresight-backend) |
| Stack | [NestJS](../../04-Stacks/nestjs.md) · TypeORM · PostgreSQL · AWS S3 · Jest · Swagger |
| Gestor de paquetes | npm |
| Ramas | `main`, `dev` (+ `dev-endpoints`, `flowCheck`, `new/crud-implement` por limpiar) |
| Puerto local | 3002 |
| Despliegue | En redefinición |

## Cómo correrlo

```bash
npm install
cp .env.example .env
npm run start:dev
```

## Estructura

```
src/
  config/namespaces/   → configuración por namespace (app, database, cognito) validada con Joi
  modules/
    assets/ custom-made/ dashboard/ me/ reports/ s3/ subscriptions/ verticals/
```

## Reglas específicas de este repo

- Módulos según [NestJS](../../04-Stacks/nestjs.md): `module`, `controller`, `service`, `dto/` y `entities/`. El controller solo orquesta.
- **Configuración nueva siempre en `src/config/namespaces/`**, con validación Joi. Es el patrón de referencia para los demás backends.
- Migraciones solo por CLI de TypeORM, con `synchronize: false`.
- Todo endpoint nuevo lleva `@ApiOperation` y `@ApiResponse`.
- Checklist de módulo nuevo: `nest g module/controller/service <nombre>`, DTOs con `class-validator` y documentación en Swagger antes de cerrar el PR.

## Variables de entorno

`NODE_ENV`, `PORT`, `API_GLOBAL_PREFIX`, `CORS_ORIGINS`, `FORESIGHT_DB_HOST`, `FORESIGHT_DB_PORT`, `FORESIGHT_DB_USER`, `FORESIGHT_DB_PASSWORD`, `FORESIGHT_DB_NAME`, `FORESIGHT_DB_SSL`, `AWS_REGION`, `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_S3_*`, `COGNITO_ISSUER`, `COGNITO_JWKS_URI`, `INNLAB_API_BASE_URL`, `INNLAB_API_TIMEOUT`, `CORE_INTERNAL_KEY`.

## Accesos necesarios

AWS (Cognito de lectura y el bucket S3 con permisos mínimos), PostgreSQL por ambiente y `CORE_INTERNAL_KEY`, que la da el dueño de InnLab Core.

## Testing

Jest configurado, **pero cero `*.spec.ts`** (ver [Testing](../../02-Testing/README.md)). Prioridad: `reports`, `subscriptions` y el cliente de la API de Core. e2e en `test/*.e2e-spec.ts`.

## Despliegue

En redefinición (ver [Despliegue y CI/CD](../../01-Estandares/despliegue-y-ambientes.md)). No hay CI.

## Deuda técnica / plan de remediación

1. El README todavía describe Keycloak, pero el código usa AWS Cognito.
2. Revisar `reports.service.ts` → `getReportPreview`: es un endpoint público sin verificación de usuario.
3. Agregar tests unitarios (hoy hay cero).
4. Relajaciones de ESLint (`no-explicit-any` y `no-floating-promises` apagadas): volver a activarlas.
5. Limpiar las ramas fuera del estándar.
6. Agregar CI cuando se defina la herramienta (lint + test + build en cada PR).
7. Agregar Husky + lint-staged + commitlint.
8. SemVer en `package.json`, tags y `CHANGELOG.md`.
9. **Sacar `.claude/settings.local.json` del repo** (está versionado) y agregar `.claude/` al `.gitignore` (ver [Documentación](../../01-Estandares/documentacion.md)).
