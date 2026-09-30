# Integrar un producto con InnLab Core

📍 [InnLab](../README.md) › [InnLab Core](README.md) › **Integrar un producto**

> **Resumen:** pides tu slug y tu key al equipo de Core, arrancas desde [product-integration-guide](product-integration-guide.md) (o copias sus piezas), configuras la validación del JWT y la recepción del SSO, y consumes `/internal/*` para el contexto de empresa. Antes de escribir lógica de negocio verificas `/health` → 200 y `/api/me/context` sin token → 401.

## Checklist en orden

- [ ] **1. Definir el `slug` del producto** (por ejemplo `indigo`) y pedirle al equipo de Core que lo registre en la tabla `Product`. Hoy es manual.
- [ ] **2. Pedir tu key interna `INTERNAL_KEY_<PRODUCTO>`.** Core la agrega en `internal.guard.ts` y te entrega el valor por un canal privado. No hay self-service todavía.
- [ ] **3. Configurar la validación del JWT** con `COGNITO_JWKS_URI` y `COGNITO_ISSUER`. Son los mismos para todo el ecosistema: **se piden a Core, no se crea otro User Pool**.
- [ ] **4. Implementar en el frontend la ruta que recibe el SSO** (`?code=`). Ver [Autenticación y SSO](autenticacion-y-sso.md).
- [ ] **5. Consumir `/internal/*`** para obtener la empresa activa, el rol, los miembros y el acceso al producto. Ver [API interna](api-interna.md).
- [ ] **6. Verificar:** `GET /health` → 200 y `GET /api/me/context` sin token → 401.
- [ ] **7. Coordinar con Core para aparecer en el hub**: card, URL del producto y origen en CORS.

## Tres formas de arrancar

| Opción | Cuándo | Qué haces |
| --- | --- | --- |
| **A. Arrancar de la plantilla** | Producto nuevo en NestJS | Clonas `product-integration-guide`, `cp .env.example .env` y agregas tus módulos directo en `src/<dominio>/`, con la [estructura estándar](../01-Estandares/estructura-de-carpetas.md) (la de indigo-backend) |
| **B. Copiar piezas** | Ya tienes un backend NestJS | Copias `src/auth/`, `src/innlab-api/`, `src/config/` y `src/health/`. Registras `AuthModule`, `InnlabApiModule` y los guards globales (`JwtAuthGuard` + `RolesGuard`) en `app.module.ts` |
| **C. Otro stack** | No es NestJS | Replicas el contrato con las librerías de tu lenguaje (ver abajo) |

### Qué tocar al copiar (opción B)

| Carpeta | Qué hace | ¿Modificar? |
| --- | --- | --- |
| `src/auth/` | Valida el JWT contra Cognito; guards y decoradores `@Public()`, `@CurrentUser()` y `@Roles()` | No, salvo que ya tengas otro sistema |
| `src/innlab-api/` | Cliente HTTP hacia `/internal/*` de Core | **Sí**: cambia `PRODUCT_SLUG` |
| `src/config/` | Carga y valida las variables de entorno | Sí: agrega tus namespaces |
| `src/modules/me/` | Ejemplo controller → service → InnlabApiService | Úsalo de plantilla o bórralo |
| `src/health/` | Liveness probe pública | No |

### Opción C: el contrato, independiente del stack

1. **Validar el JWT** contra el JWKS de Cognito, con algoritmo `RS256` y `issuer === COGNITO_ISSUER`. Sirve cualquier librería JWT + JWKS (Python: `python-jose`; Go: `lestrrat-go/jwx`; Node: `jsonwebtoken` + `jwks-rsa`).
2. **Rechazar si `token_use !== 'access'`**, para que no se pueda usar un id_token donde se espera un access_token.
3. **Llamar a `/internal/*` con el header `x-internal-key`.** Son 6 llamadas simples y sin estado.
4. **Recibir el SSO** en el frontend, como se describe en [Autenticación y SSO](autenticacion-y-sso.md).

## Variables de entorno de un producto

```
# Validación de tokens (se piden a Core, son iguales para todos)
COGNITO_JWKS_URI=
COGNITO_ISSUER=

# Llamadas salientes a Core
INNLAB_API_BASE_URL=
INNLAB_API_TIMEOUT=
CORE_INTERNAL_KEY=          # tu INTERNAL_KEY_<PRODUCTO>

# Frontend
VITE_CORE_URL=              # hub de Core
VITE_CORE_API_URL=          # API de Core
```

La base de datos del producto es **propia**: `DB_*`, nunca la de Core.

## Errores comunes

- **Confundir `token` (id_token) con `accessToken`.** En el header `Authorization` va solo el `accessToken`.
- **El código SSO expira en 30 segundos.** Si el canje falla, se reinicia el flujo desde el hub; no se reintenta el mismo código.
- **Llamar a Core en cada request para validar el token.** No hace falta: el JWT se valida localmente contra el JWKS, que queda en caché.
- **Formato de la key:** algunos comentarios del código mencionan `<service>:<secret>`, pero los valores reales son hex plano.
- **Roles por empresa (`companyRole`):** el JWT no los trae. Ver [Estado y pendientes](estado-y-pendientes.md).

## Qué NO copiar de Foresight

La lógica de negocio (`assets`, `reports`, `subscriptions`, `verticals`, `custom-made`, `dashboard`), su módulo `s3/` ni ningún `.env` real. Las credenciales de Foresight no se reutilizan.
