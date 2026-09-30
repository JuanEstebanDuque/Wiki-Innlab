# Estructura de carpetas

📍 [InnLab](../README.md) › [Estándares](README.md) › **Estructura de carpetas**

> **Resumen:** hay una estructura base para backend y otra para frontend, ambas **organizadas por dominio o feature**. Son una **guía**: se exige un mínimo (en backend: `config/`, `common/`, `auth/` y módulos por dominio) y el resto de carpetas se crea solo si el proyecto las usa. En repos nuevos se sigue desde el inicio. En los repos existentes se adopta **de forma gradual**, sin reestructurar todo de golpe: el código nuevo va en la ubicación estándar, y mover lo existente se hace en PRs de refactor pequeños y separados.

## Backend

Basada en **indigo-backend**. Es una guía, no una lista cerrada.

```
src/
  main.ts
  app.module.ts
  config/                → lectura y validación de las variables de entorno
  common/                → lo que usan varios módulos (según lo que haga falta:
                           decorators/, filters/, guards/, interceptors/, middleware/…)
  auth/                  → autenticación con InnLab Core (de product-integration-guide)
  <modulo>/              → un módulo por dominio, directo en src/ (ej. diagnosticador/)
    <modulo>.module.ts
    <modulo>.controller.ts
    <modulo>.service.ts
    dto/                 → si el módulo recibe datos
    entities/            → si el módulo tiene tablas en la BD
    interfaces/          → si el módulo tiene tipos propios
    …                    → otras carpetas que el módulo necesite (ej. search/, generate/, database/)
```

**Mínimo en todo backend:** `src/` con `config/`, `common/`, `auth/` y los módulos. Lo demás es una guía: se crea solo si el proyecto lo usa.

- **Módulos:** uno por dominio de negocio, directo en `src/`. Nada de agrupar por tipo de archivo (un `controllers/` global con todo).
- **Dentro de cada módulo**, las carpetas habituales son `dto/`, `entities/` e `interfaces/`, pero solo las que el módulo necesite. Puede tener otras: por ejemplo, `diagnosticador/` en indigo-backend separa su lógica en `search/`, `generate/` y `database/`.
- **`common/`** igual: se crean solo las subcarpetas que se usen, separadas por tipo.
- Un módulo no entra a los archivos internos de otro: usa su service exportado.
- Los tests (`.spec.ts`) van al lado del archivo que prueban.

Detalle y ejemplos en [NestJS](../04-Stacks/nestjs.md).

## Frontend (React y Vue)

```
src/
  main.ts / main.tsx     → arranque
  App.vue / App.tsx
  router/                → rutas, incluida la que recibe el SSO de Core
  modules/
    <feature>/           → una carpeta por funcionalidad (reports, diagnostics, billing…)
      components/        → componentes solo de esta feature
      pages/ o views/    → pantallas (React: pages, Vue: views)
      hooks/ o composables/
      index.ts           → lo único que otras features pueden importar
  components/ui/         → componentes genéricos reutilizables (botones, inputs, modales)
  services/              → clientes HTTP, uno por API (core.service.ts, <producto>.service.ts)
  hooks/ o composables/  → lógica reutilizable global (useAuth…)
  context/ o stores/     → estado global, solo si hace falta
  config.ts              → lectura centralizada de variables VITE_*
  assets/
```

**Reglas:** lo que es de una sola feature vive dentro de su carpeta. Lo que usan dos o más features sube a `components/ui/`, `services/` o `hooks|composables/`. Las llamadas HTTP solo se hacen desde `services/`. Una feature no importa archivos internos de otra, solo su `index.ts`.

Detalle en [React + Vite](../04-Stacks/react-vite.md) y [Vue + Vite](../04-Stacks/vue-vite.md). Las landings pequeñas pueden organizarse por tipo (`pages/`, `components/`), lo que se declara como excepción en la página del repo.

## Cómo se adopta sin romper los proyectos existentes

Reestructurar todo de una vez **no se hace**. Genera conflictos con todas las ramas abiertas, PRs imposibles de revisar, imports rotos, y mezcla cambios de estructura con cambios de lógica.

| Situación | Regla |
| --- | --- |
| **Repo nuevo** | Estructura base desde el primer commit |
| **Módulo o feature nueva en un repo existente** | Se crea ya en la ubicación estándar |
| **Archivo existente que vas a modificar** | Se puede mover, pero **en un PR aparte** de tipo `refactor:`, sin cambios de lógica, antes o después del PR funcional |
| **Mover muchos archivos** | Se coordina con el equipo, se hace cuando haya pocas ramas abiertas y en PRs pequeños (una carpeta o feature por PR) |
| **Proyecto que se va a reemplazar** (Growinn) | No se reestructura |

**Nunca en el mismo PR:** mover archivos **y** cambiar su lógica. El diff se vuelve ilegible y el revisor no puede ver qué cambió de verdad.

## Qué tan lejos está cada repo

| Repo | Distancia al estándar | Qué implica |
| --- | --- | --- |
| indigo-backend | ✅ Es la referencia de backend | Le falta `auth/`, que llega con la integración a Core |
| product-integration-guide | 🟢 Cerca | Es la referencia para `auth/`. Su módulo de ejemplo está en `modules/me/`; se acepta |
| innlab-core-api | 🟢 Cerca | Faltan `config/` y `common/`, y darles módulo a las entidades sueltas |
| foresight-backend | 🟢 Cerca | Ya está por dominio. Sus módulos están dentro de `modules/`; se acepta y no hace falta moverlos |
| foresight-user-space | 🟢 Cerca | Ya es por feature (`app/`, `modules/`, `services/`) |
| innlab-core-user-space | 🟡 Media | Está por tipo (`pages/`, `components/`). Es un repo pequeño: se puede migrar por partes |
| indigo-user-space | ✅ Recién creado | Se arma directamente con la estructura estándar |
| foresight-landing | ⚪ Excepción | Landing organizada por tipo; se acepta |
| foresight-scrapping | 🟡 Media | Estructura objetivo `notebooks/` + `src/` + `tests/` (ver [Python](../04-Stacks/python-notebooks.md)) |
| -Growinn-index-test | — | No se reestructura (se reemplaza por Índigo) |

La página de cada repo describe su estructura **actual**, para que nadie se pierda mientras dura la transición.
