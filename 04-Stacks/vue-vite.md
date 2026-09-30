# Vue + Vite (frontend)

📍 [InnLab](../README.md) › [Stacks](README.md) › **Vue + Vite**

> **Resumen:** Vue 3 con Composition API (`<script setup lang="ts">`), TypeScript y Vite. Las llamadas HTTP van solo desde `services/`, la lógica reutilizable en composables, las variables `VITE_*` son por ambiente y la autenticación va por el SSO de InnLab Core.

**Repos:** [indigo-user-space](../05-Proyectos/indigo/indigo-user-space.md)

**Por qué Vue en Índigo:** se eligió por velocidad de desarrollo, rendimiento y una curva de aprendizaje fácil para todo el equipo. Es una decisión tomada, y el frontend de Índigo se queda en Vue.

## Estructura de carpetas

Base común del frontend: [Estructura de carpetas](../01-Estandares/estructura-de-carpetas.md). Cómo adoptarla en repos existentes sin reestructurar todo de golpe también está ahí.
 (por feature)

```
src/
  main.ts               → createApp, router, plugins
  App.vue
  router/               → rutas (incluida la ruta que recibe el SSO de Core)
  modules/<feature>/    → components/, views/, composables/, index.ts (API pública del módulo)
  components/ui/        → componentes genéricos reutilizables (BaseButton, BaseInput…)
  composables/          → lógica reutilizable global (useAuth, useApi…)
  services/             → clientes HTTP (uno por API: indigo.service.ts, core.service.ts)
  stores/               → estado global, solo si hace falta
  config.ts             → lectura centralizada de las variables VITE_*
  assets/
```

- Un módulo **no importa archivos internos de otro módulo**, solo su `index.ts`.

## Reglas de código

- **Composition API con `<script setup lang="ts">`** en todos los componentes. No se mezcla con Options API.
- **Un componente = una responsabilidad.** La lógica de datos o de estado se extrae a un **composable** (`useDiagnostic.ts`).
- **Nombres:** componentes en PascalCase y con más de una palabra (`DiagnosticCard.vue`, no `Card.vue`); composables en camelCase con prefijo `use`; componentes base con prefijo `Base` (`BaseButton.vue`).
- **Props y emits tipados** (`defineProps<...>()`, `defineEmits<...>()`).
- **Las llamadas HTTP solo se hacen desde `services/`**, nunca dentro de un componente.
- **Variables de entorno:** solo `VITE_*`, leídas en `config.ts`. **No contienen secretos**, porque terminan en el bundle público.
- **Autenticación solo por InnLab Core:** una ruta del router recibe `?code=`, canjea el código y guarda el `accessToken`. No hay login propio. Ver [Autenticación y SSO](../03-InnLab-Core/autenticacion-y-sso.md).
- Estado global solo cuando de verdad se comparte entre vistas (la opción habitual en Vue es Pinia). Si no hace falta, un composable basta.

## Tests

**Qué probar:** [Testing → Frontend](../02-Testing/que-probar-back-y-front.md).

Framework libre, con las [convenciones comunes](../02-Testing/convenciones.md). Lo habitual en Vue + Vite es Vitest con Vue Test Utils. `NombreComponente.test.ts` va junto al componente. Prioridad: composables, `services/` y la recepción del SSO.

## Scripts estándar

| Script | Qué hace |
| --- | --- |
| `dev` | Servidor de desarrollo |
| `build` | `vue-tsc -b && vite build` |
| `preview` | Sirve el build localmente |
| `lint` / `format` | ESLint (con `eslint-plugin-vue`) y Prettier |
| `test` | Tests |

## Despliegue

El build (`dist/`) es un sitio estático, y las rutas de la SPA necesitan un *rewrite* `/* → /index.html`. La plataforma está en definición (ver [Despliegue y CI/CD](../01-Estandares/despliegue-y-ambientes.md)).
