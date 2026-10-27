# Git y ramas

📍 [InnLab](../README.md) › [Estándares](README.md) › **Git y ramas**

> **Resumen:** `main` es producción y `dev` es integración. Cada tarea se trabaja en su propia rama `<tipo>/<issue>-<descripcion>`, sale de `dev`, vuelve a `dev` por PR y se borra al hacer merge.

## Ramas permanentes

| Rama | Rol | Quién escribe en ella |
| --- | --- | --- |
| `main` | Lo que está en producción | Solo la promoción automática desde `dev` y los PR de `hotfix/*` |
| `dev` | Integración: lo siguiente que va a salir | Solo merges de PRs de ramas de trabajo |

- **Todo repo tiene exactamente estas dos ramas desde su creación:** `main` (nunca `master`) y `dev`. Al crear un repo, lo primero es crear `dev` a partir de `main` y protegerlas.

**Estado actual:**

| Repo | Falta |
| --- | --- |
| indigo-backend | Renombrar `master` → `main` |
| indigo-user-space | Primer commit en `main` y crear `dev` |
| innlab-core-user-space | Crear `dev` |
| product-integration-guide | Crear `dev` |
| foresight-scrapping | Crear `dev` |
| -Growinn-index-test | Crear `dev` (se mantiene tal cual, pero la regla aplica si recibe cambios) |
- Las dos ramas deben estar **protegidas** en GitHub: sin push directo, PR con al menos 1 aprobación y checks del CI en verde. **Hoy no se puede** (el plan gratuito de GitHub no protege ramas en repos privados), así que se cumple como regla de equipo: ver [CI y calidad › Sin protección de ramas](ci-y-calidad.md#sin-protección-de-ramas-situación-actual).
- **Ningún PR de trabajo va directo a `main`**: si algo entra a `main` sin pasar por `dev`, la promoción choca (el simulacro lo detecta).

## Ramas de trabajo

Formato: **`<tipo>/<issue>-<descripcion-corta>`**

| Tipo | Cuándo | Sale de | Vuelve a |
| --- | --- | --- | --- |
| `feat/` | Funcionalidad nueva | `dev` | `dev` |
| `fix/` | Bug que no es urgente en producción | `dev` | `dev` |
| `hotfix/` | Bug crítico que ya está en producción | `main` | `main` **y** `dev` |
| `refactor/` | Cambio interno sin alterar comportamiento | `dev` | `dev` |
| `docs/` | Solo documentación | `dev` | `dev` |
| `chore/` | Configuración, dependencias, tooling | `dev` | `dev` |

Ejemplos: `feat/42-pdf-export`, `fix/57-empty-vertical-list`, `hotfix/61-cognito-token-refresh`.

**Reglas**
- Todo en **kebab-case** y en inglés, sin mayúsculas ni espacios.
- El número de issue va siempre que exista issue, y para `feat` y `fix` debería existir siempre (ver [Issues y PRs](issues-y-pull-requests.md)).
- **Una rama = una tarea.** No se acumulan historias en la misma rama.
- La rama se **borra** (local y remota) al hacer merge del PR. GitHub puede hacerlo solo: *Settings → Automatically delete head branches*.
- Antes de abrir el PR, actualiza tu rama con `dev` (`git pull --rebase origin dev`).

**Se usa `feat/` y no `feature/`**, para que coincida con el tipo de commit. Hoy conviven varios formatos porque la wiki todavía no se ha socializado con los equipos. Desde su publicación, las ramas nuevas siguen esta tabla y las existentes se limpian a medida que se hace merge de ellas.

## Nombres que no se usan

Estos nombres existen hoy en los repos. **No se crean ramas nuevas así**, y las existentes se limpian:

| Ejemplo actual | Problema |
| --- | --- |
| `flowCheck`, `cdg`, `responsive` | No dicen qué tipo de cambio es ni a qué tarea corresponde |
| `new/crud-implement`, `styles/components`, `dev-endpoints` | Prefijos fuera del estándar |
| `feat/Pago-Amedida`, `feat/connect-Json-to-US` | Mayúsculas y mezcla de idiomas |

## Releases y tags

Un release es un PR de `dev` a `main` titulado `release: vX.Y.Z`, con tag y CHANGELOG. Cómo se decide el número (0.1.0, 1.0.0…) y el proceso completo están en **[Versionado y releases](versionado-y-releases.md)**.

## ¿Por qué se borran las ramas después del merge?

Es la práctica estándar de la industria (GitHub Flow, GitFlow). No se pierde nada: los commits quedan en `dev`, y el PR conserva la discusión, el diff y el nombre de la rama. GitHub permite restaurarla desde el PR cerrado ("Restore branch"). Dejarlas vivas genera ramas huérfanas de las que nadie sabe si siguen en uso. Las únicas ramas permanentes son `main` y `dev`.
