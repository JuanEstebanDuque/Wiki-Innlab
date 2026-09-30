# innlab-core-user-space

📍 [InnLab](../README.md) › [InnLab Core](README.md) › **innlab-core-user-space**

> **Resumen:** frontend de InnLab Core, en React 19 + Vite + TypeScript + Tailwind. Tiene la landing, el registro y login con Cognito, el onboarding y el hub de productos, que es el que inicia el SSO hacia cada producto.

## Ficha

| | |
| --- | --- |
| Repo | [innlab2016/innlab-core-user-space](https://github.com/innlab2016/innlab-core-user-space) |
| Stack | [React + Vite](../04-Stacks/react-vite.md) · **TypeScript** · Tailwind · `amazon-cognito-identity-js` |
| Ramas | `main` (+ `flowCheck` por limpiar). **Falta `dev`** |
| Puerto local | 5173 |

## Cómo correrlo

```bash
npm install
# crear .env con las variables de abajo
npm run dev
```

## Estructura

```
src/
  pages/        → Landing, Login, Register, Verify, ForgotPassword, ResetPassword,
                  NewPassword, Onboarding, Hub, Session, Sso
  components/   → hub/ (ProductCard, ProductCardLocked), ui/ (Button, Footer, NavBar)
  context/      → AuthContext
  hooks/        → useAuth
  services/     → core.service.ts
```

**Es el repo de referencia para frontends nuevos** (TypeScript, con `services/`, `context/` y `hooks/` separados).

## Reglas específicas de este repo

- Agregar un producto al hub: su entrada en `PRODUCT_CONFIG` y su imagen en `public/images/<producto>-card.png`, hasta que exista el catálogo de productos (ver [Estado y pendientes](estado-y-pendientes.md)).
- Las URLs de los productos no se hardcodean por ambiente: salen de variables `VITE_*`.

## Variables de entorno

`VITE_API_URL`, `VITE_COGNITO_USER_POOL_ID`, `VITE_COGNITO_CLIENT_ID`.

## Testing

No hay. Prioridad: `AuthContext`/`useAuth`, `SsoPage` y el registro y la verificación.

## Deuda técnica / plan de remediación

1. Reemplazar el README de la plantilla de Vite.
2. Crear `.env.example`.
3. Crear la rama `dev` y protegerla junto con `main`.
4. Sacar las URLs de productos de `PRODUCT_CONFIG` a variables de entorno (se hicieron 3 commits para cambiar a mano la URL de Foresight).
5. Nombres de archivo inconsistentes en `ui/` (`navbar.css` junto a `NavBar.tsx`).
6. Agregar tests.
