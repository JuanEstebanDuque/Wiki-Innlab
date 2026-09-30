# product-integration-guide

📍 [InnLab](../README.md) › [InnLab Core](README.md) › **product-integration-guide**

> **Resumen:** plantilla NestJS con **solo la capa de integración con InnLab Core**, extraída de la que ya funciona en producción en `foresight-backend`, sin la lógica de negocio de Foresight ni sus credenciales. Es el punto de partida obligatorio para cualquier backend de producto nuevo.

## Ficha

| | |
| --- | --- |
| Repo | [innlab2016/product-integration-guide](https://github.com/innlab2016/product-integration-guide) |
| Stack | [NestJS](../04-Stacks/nestjs.md) · Passport JWT + `jwks-rsa` · `@nestjs/axios` · Jest |
| Ramas | `main` (1 commit) |

## Cómo usarlo

```bash
npm install
cp .env.example .env   # los valores reales los da el equipo de Core
npm run start:dev
# GET /health            → 200 { status: 'ok' }
# GET /api/me/context    → 401 sin token (el guard global está activo)
```

La guía completa está en [Integrar un producto](integrar-un-producto.md).

## Estructura

```
src/
  auth/
    strategies/jwt.strategy.ts       → valida el JWT contra el JWKS de Cognito
    guards/jwt-auth.guard.ts         → guard global
    guards/roles.guard.ts
    decorators/                      → @Public(), @CurrentUser(), @Roles()
  innlab-api/innlab-api.service.ts   → cliente de /internal/* (cambiar PRODUCT_SLUG)
  config/namespaces/                 → app, database, cognito, innlab-api (validados)
  health/                            → liveness probe pública
  modules/me/                        → módulo de ejemplo
```

## Reglas de este repo

- **Es una plantilla, no un producto:** no lleva lógica de negocio.
- Lo que un producto construya y sirva para todos (por ejemplo, el interceptor de `companyRole`) **se sube aquí** para que el siguiente equipo no lo repita.
- Cualquier cambio en el contrato de Core se refleja aquí y en [API interna](api-interna.md) en el mismo PR.

## Pendientes

1. `GUIDE.md` enlaza a un artifact externo como guía principal. Pasar a enlazar esta wiki, que es la fuente de verdad.
2. Agregar tests reales de `/health` y del guard (hoy solo está el e2e de la plantilla).
3. Crear la rama `dev` y protegerla.
