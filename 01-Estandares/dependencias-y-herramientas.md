# Dependencias y herramientas

📍 [InnLab](../README.md) › [Estándares](README.md) › **Dependencias y herramientas**

> **Resumen:** **no se impone herramienta** (npm o pnpm, Jest o Vitest…). Sí se exige que cada repo sea reproducible (**versiones exactas, nunca "latest"**, lockfile y runtime fijados), que tenga lint y formato automáticos, y que cada dependencia nueva se justifique.

## Lo que se exige (sin importar la herramienta)

| Regla | Por qué |
| --- | --- |
| **Un solo gestor de paquetes por repo**, con su lockfile versionado (nunca dos lockfiles) | Instalaciones reproducibles |
| La **versión del runtime fijada** (`.nvmrc` y `"engines"` en Node; `requires-python` o `.python-version` en Python) | Mismo comportamiento en todas las máquinas y en CI |
| El **README dice qué gestor se usa** | Nadie tiene que adivinar |
| **Scripts con nombres estándar:** `dev`/`start:dev`, `build`, `lint`, `format` y `test` | El CI y las personas nuevas corren cualquier repo igual |
| **Lint y formato configurados** y corriendo sin errores en `main` | Código consistente sin discutirlo en cada review |

**Lint en los repos TS/JS: ESLint** (lo usan todos; `indigo-backend` migra de oxlint a ESLint). **Formato: Prettier** con `{ "singleQuote": true, "trailingComma": "all" }`.

Reglas de lint que no se desactivan en código nuevo: `no-explicit-any` (como mínimo `warn`) y, en backend, `no-floating-promises`.

## Versiones fijadas

Nada se instala como "la última versión": una instalación de hoy y una de dentro de seis meses deben dar exactamente lo mismo. Todo se actualiza con un PR que pasa el CI.

| Qué | Cómo se fija | Ejemplo |
| --- | --- | --- |
| Dependencias del proyecto | Versión **exacta** en el manifiesto, sin `^` ni `~`. En npm/pnpm, `save-exact=true` en `.npmrc` para que las nuevas también entren exactas | `"zod": "4.6.5"` |
| Lockfile | Versionado, y el CI instala sin modificarlo (`npm ci`, `pnpm install --frozen-lockfile`) | — |
| Gestor de paquetes | Campo `packageManager` en `package.json` (o su equivalente) | `"packageManager": "pnpm@11.3.0"` |
| Runtime | Versión completa en `.nvmrc` o `.python-version` | `24.13.1` |
| GitHub Actions | Por **SHA del commit**, con la versión en un comentario. Una etiqueta como `@v4` puede moverse | `actions/checkout@11d5960… # v4.4.0` |
| Imágenes de Docker y servicios del CI | Con su versión de parche, nunca `latest` | `postgres:16.15-alpine` |
| Runner del CI | Versión explícita | `ubuntu-24.04` |

**Actualizaciones:** cada repo tiene `.github/dependabot.yml`, que abre PRs semanales contra `dev`. Los parches y menores van agrupados en un solo PR, y cada major en uno propio. Se fusionan como cualquier PR: CI en verde y revisión. El runtime (`.nvmrc`) se sube a mano.

## Agregar una dependencia

Antes de agregar una librería, responde en el PR:
- ¿Se puede hacer con lo que ya hay, o con la librería estándar?
- ¿Está mantenida? (último release, issues abiertos)
- ¿Su tamaño y su licencia son aceptables?

Reglas:
- Toda librería que se importa en el código está **declarada** en el manifiesto (`package.json`, `requirements.txt`).
- Las que solo se usan en desarrollo van en `devDependencies`.
- No se instalan paquetes que reemplazan módulos nativos (`fs`, `path` o `https` desde npm).
- Las actualizaciones mayores (major) van en un PR propio.

## Hooks de git (Propuesta)

Con Husky + lint-staged + commitlint (o su equivalente):
- `pre-commit`: formato y lint de los archivos modificados.
- `commit-msg`: valida el formato de [Commits](commits.md).

## Editor

- **(Propuesta)** `.editorconfig` común: UTF-8, LF y 2 espacios.
- La configuración personal del editor no se versiona.
