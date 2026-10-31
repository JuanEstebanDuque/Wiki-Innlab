# indigo-user-space

📍 [InnLab](../../README.md) › [Proyectos](../README.md) › [Índigo](README.md) › **indigo-user-space**

> **Resumen:** frontend de Índigo en **Vue 3 + Vite + TypeScript**, con el portal del profesor (conectado al backend) y el portal empresa. Inicia sesión por el SSO de InnLab Core, usa el color naranja del design system de iNNLAB y tiene CI con pruebas unitarias y **E2E con Playwright**: es el **repo de referencia de frontend** para el estándar de [CI y calidad](../../01-Estandares/ci-y-calidad.md). Vue se eligió por velocidad de desarrollo, rendimiento y curva de aprendizaje, y **así se queda**.

## Ficha

| | |
| --- | --- |
| Repo | [innlab2016/indigo-user-space](https://github.com/innlab2016/indigo-user-space) |
| Stack | [Vue + Vite](../../04-Stacks/vue-vite.md) · Vue 3 · Vue Router · TypeScript (`vue-tsc`) · Vitest · Playwright · CSS plano |
| Versiones | npm · Node 24.13.1 (`.nvmrc`) · dependencias exactas |
| Lint | ESLint sin advertencias (`--max-warnings 0`) |
| Ramas | `main` y `dev` |
| Autenticación | SSO de InnLab Core: callback en `/auth/callback`, token en `sessionStorage` y guard de rutas por rol |
| Diseño | Figma **INDIGO** (InnLab); tokens en `src/style.css` y guía en `DESIGN.md` |
| Despliegue | Aún no: plataforma en definición |

## Cómo correrlo

```bash
npm install
cp .env.example .env   # los valores los da el responsable del proyecto
npm run dev            # http://localhost:5174 (el 5173 es del hub de InnLab Core)
```

- Con `VITE_AUTH_DEV=true` entra sin InnLab Core y el backend responde como su usuario de desarrollo.
- Con `VITE_AUTH_DEV=true` y `VITE_DEMO_EMPRESA=true` entra al portal empresa con una dueña de ejemplo, sin backend.

## Pantallas

| Portal | Rutas |
| --- | --- |
| Profesor | Panel de control, diagnosticadores (lista, detalle y asistente de creación), análisis comparativo, reporte por empresa y protocolo académico |
| Empresa (owner/admin) | Inicio, catálogo y solicitud de aplicación, aplicaciones, resultados (Free y Premium), notificaciones y configuración con plan y método de pago |

El sidebar es el del Figma (colapsable a 57px) y cada portal define su menú en `src/components/dashboard/menus.ts`.

## Testing y CI

```bash
npm test          # unitarias y de componentes (Vitest)
npm run test:cov  # con cobertura; falla bajo 70 % de líneas
npm run test:e2e  # Playwright: flujos críticos contra el build de producción, con Core y la API simuladas
npm run lint
```

- **CI** (`.github/workflows/ci.yml`):
  1. Auditoría de dependencias de producción.
  2. Lint, pruebas con cobertura y build.
  3. Job `e2e` con Playwright: sesión, acceso por rol, panel del profesor y flujos del portal empresa.
- `smoke.yml` revisa un ambiente desplegado y `promocion.yml` hace el simulacro `dev` → `main`.
- Dependabot abre PRs semanales contra `dev`.
- SonarQube Cloud no se usa en los frontends (ver [CI y calidad](../../01-Estandares/ci-y-calidad.md#sonarqube-cloud-solo-backends)).

## Pendientes

1. **Conectar el portal empresa al backend.** Hoy sus pantallas usan datos de ejemplo en memoria (`src/empresa/datos.ts` y `estado.ts`), aunque los endpoints ya existen en indigo-backend: hay que cambiar las acciones de `estado.ts` por llamadas a `src/api/indigo.ts`.
2. **Buscador y notificaciones de la barra superior del portal profesor:** se ven, pero todavía no hacen nada.
