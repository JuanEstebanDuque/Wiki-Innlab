# foresight-user-space

📍 [InnLab](../../README.md) › [Proyectos](../README.md) › [Foresight](README.md) › **foresight-user-space**

> **Resumen:** aplicación del cliente de Foresight, en React 19 + Vite + Tailwind y JavaScript. El usuario llega por SSO desde InnLab Core y ve sus informes, radares, planes y pagos.

## Ficha

| | |
| --- | --- |
| Repo | [innlab2016/foresight-user-space](https://github.com/innlab2016/foresight-user-space) |
| Stack | [React + Vite](../../04-Stacks/react-vite.md) · **JavaScript** · Tailwind |
| Gestor de paquetes | npm |
| Ramas | `main`, `dev` + 11 ramas `feat/*` y otras por limpiar |
| Puerto local | 5174 |
| Despliegue | En redefinición |

## Cómo correrlo

```bash
npm install
# crear .env con las variables de abajo
npm run dev
```

## Estructura (por feature)

```
src/
  app/                 → layouts, providers, router
  modules/<feature>/   → components, pages, mocks, index.js
  services/            → auth, foresight, innlab
  components/ui/       → UI global
```

## Reglas específicas de este repo

- Se mantiene en **JavaScript**. No se agregan `.tsx` sueltos: si se migra a TS, se migra todo.
- Las llamadas a Foresight van por `services/foresight`, y las de Core por `services/innlab`.
- Los mocks de `modules/<feature>/mocks` no se usan en producción: se borran cuando el endpoint real exista.

## Variables de entorno

`VITE_FORESIGHT_API_URL`, `VITE_CORE_API_URL`, `VITE_CORE_URL`.

## Testing

No hay. Empezar por `services/`, la recepción del SSO y los hooks con lógica (ver [Testing](../../02-Testing/README.md)).

## Deuda técnica / plan de remediación

1. `HeroCards.tsx` huérfano en un proyecto JS sin `tsconfig.json`.
2. Carpetas duplicadas `UIbuttons/` y `buttons/`: unificarlas.
3. Normalizar los nombres de archivo (`footer.jsx`, `navbar.jsx` → PascalCase).
4. Traducir los nombres en español (`pagosAmedida.jsx`) y corregir typos.
5. Limpiar las ramas (`feat/Pago-Amedida`, `feat/connect-Json-to-US`, `styles/components`, `flowCheck`…).
6. Agregar tests.
7. Agregar Prettier + Husky + lint-staged.
8. Crear `.env.example`.
