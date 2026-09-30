# React + Vite (frontend)

📍 [InnLab](../README.md) › [Stacks](README.md) › **React + Vite**

> **Resumen:** son componentes funcionales, con TypeScript en los repos nuevos, Tailwind para estilos, llamadas HTTP solo desde `services/`, lógica en hooks y variables `VITE_*` por ambiente.

**Repos:** [innlab-core-user-space](../03-InnLab-Core/innlab-core-user-space.md) · [foresight-user-space](../05-Proyectos/foresight/foresight-user-space.md) · [foresight-landing](../05-Proyectos/foresight/foresight-landing.md) · [Growinn Frontend](../05-Proyectos/growinn-index/growinn-index-test.md)

## Lenguaje

- **Repos nuevos: TypeScript** (`.tsx`/`.ts`). Así está `innlab-core-user-space` y es la referencia.
- Los repos existentes en JS siguen en JS. **No se mezclan** archivos `.tsx` sueltos en un repo JS: si se migra, se migra completo y con `tsconfig.json`.

## Estructura de carpetas (por feature)

```
src/
  app/                  → router, providers, layouts
  modules/<feature>/    → components/, pages/, hooks/, index.ts (API pública del módulo)
  components/ui/        → componentes genéricos reutilizables (Button, Input, Modal…)
  services/             → clientes HTTP (uno por API: core.service.ts, foresight.service.ts)
  hooks/                → hooks globales (useAuth…)
  context/              → contextos globales (AuthContext…)
  constants/            → configuración estática
  assets/
```

- Un módulo **no importa archivos internos de otro módulo**, solo su `index.ts`.
- Para landings o sitios pequeños se acepta la estructura por tipo (`pages/`, `components/`, `constants/`), como en `foresight-landing`.

## Reglas de código

- **Un componente = una responsabilidad.** Si tiene lógica de datos, se extrae a un hook.
- El **archivo se llama igual que el componente**, en PascalCase: `ProductCard.tsx`. Si el componente tiene carpeta, la carpeta también: `NavBar/NavBar.tsx`.
- **Las llamadas HTTP solo se hacen desde `services/`**, nunca con `fetch` o `axios` dentro de un componente.
- **Estilos: Tailwind.** Un CSS por componente solo cuando Tailwind no alcanza. No se mezclan tres sistemas de estilos en un repo.
- **Variables de entorno:** solo `VITE_*`, leídas en un único `config.ts`. **No contienen secretos**, porque todo lo que es `VITE_*` termina en el bundle público.
- **No quedan componentes stub vacíos** (archivos de 0 bytes) en el repo.
- **Autenticación:** solo a través de InnLab Core. El frontend tiene una ruta que recibe el SSO (`?code=`) y no tiene login propio. Ver [Autenticación y SSO](../03-InnLab-Core/autenticacion-y-sso.md).

## Testing

**Qué probar:** [Testing → Frontend](../02-Testing/que-probar-back-y-front.md).

- Framework libre (por ejemplo Vitest o Jest con Testing Library), con las [convenciones comunes](../02-Testing/convenciones.md). Hoy ningún frontend tiene tests.
- `Componente.test.tsx` junto al componente.
- Prioridad: `services/`, hooks con lógica, formularios y flujos de varios pasos.

## Comandos estándar

| Script | Qué hace |
| --- | --- |
| `dev` | Servidor de desarrollo |
| `build` | Build de producción (en TS: `tsc -b && vite build`) |
| `preview` | Sirve el build localmente |
| `lint` / `format` | ESLint y Prettier |
| `test` | Tests |

## Despliegue

El build (`dist/`) es un sitio estático. Las rutas de la SPA necesitan un *rewrite* `/* → /index.html`. La plataforma está en definición: ver [Despliegue y CI/CD](../01-Estandares/despliegue-y-ambientes.md).
