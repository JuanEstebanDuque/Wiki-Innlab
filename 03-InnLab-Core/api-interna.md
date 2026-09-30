# API interna de Core

📍 [InnLab](../README.md) › [InnLab Core](README.md) › **API interna**

> **Resumen:** Core recibe peticiones de dos tipos: las del **frontend** de los productos (endpoints públicos de auth, con el token del usuario) y las del **backend** de los productos (`/internal/*`, con una key por backend). La key identifica qué backend hace la petición, para que no se pueda llamar desde cualquier lado.

## Keys por backend

Cada backend de producto tiene **su propia key**, y Core la mapea al nombre del servicio (`internal.guard.ts`):

| Servicio | Variable en Core | Producto |
| --- | --- | --- |
| `foresight-service` | `INTERNAL_KEY_FORESIGHT` | Foresight |
| `inndex-service` | `INTERNAL_KEY_INNDEX` | Inndex (Growinn) |
| `ucorp-service` | `INTERNAL_KEY_UCORP` | Ucorp |
| `diag-irl-service` | `INTERNAL_KEY_DIAG_IRL` | diag-irl |

Reglas:
- La key **solo vive en el backend** (`CORE_INTERNAL_KEY` en su `.env`). Nunca va en un frontend ni en una variable `VITE_*`.
- **No se comparte entre productos.** Cada producto nuevo pide la suya.
- Si una key se filtra, se rota en Core y en el producto (ver [Seguridad](../01-Estandares/seguridad-y-accesos.md)).

## Endpoints `/internal/*`

Header obligatorio: `x-internal-key: <key del producto>`.

| Método y ruta | Devuelve |
| --- | --- |
| `GET /internal/users/:userId/context` | Empresa activa, rol del usuario y lista de sus empresas |
| `GET /internal/users/:userId/features?product=<slug>` | Feature flags del usuario en el producto (solo lectura) |
| `GET /internal/companies/:companyId` | Datos básicos de la empresa (nombre) |
| `GET /internal/companies/:companyId/members` | Miembros activos de la empresa |
| `GET /internal/companies/:companyId/product-access?product=<slug>` | Si la empresa tiene el producto activo |
| `POST /internal/roles/assign` | ⚠️ **Stub**: responde `{ assigned: true }`, pero no guarda nada todavía |

En NestJS, todas estas llamadas ya están implementadas en `InnlabApiService` de [product-integration-guide](product-integration-guide.md).

## Endpoints públicos (desde el frontend)

| Método y ruta | Uso |
| --- | --- |
| `GET /auth/sso/exchange?code=` | Canjear el código SSO por tokens |
| `POST /auth/logout` | Logout centralizado |
| `GET /auth/introspect` | Verificar si la sesión sigue activa |

Detalle del flujo en [Autenticación y SSO](autenticacion-y-sso.md).

## Cambios en este contrato

Esta API la usan todos los productos. Cualquier cambio que rompa compatibilidad:
- Se marca como `feat!:` con `BREAKING CHANGE:` (ver [Commits](../01-Estandares/commits.md)).
- Se avisa antes a los equipos de los productos conectados.
- Se actualiza esta página en el mismo PR.
