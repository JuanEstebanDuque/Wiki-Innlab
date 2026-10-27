# CI y calidad

📍 [InnLab](../README.md) › [Estándares](README.md) › **CI y calidad**

> **Resumen:** todo repo corre en **GitHub Actions**, en los servidores que pone GitHub (no hay servidor propio que mantener). Cada PR pasa los mismos checks: instalación reproducible, lint sin advertencias, build, pruebas con su cobertura mínima y auditoría de dependencias. Los **backends** además pasan por **SonarQube Cloud**. Los comandos cambian según la tecnología; las reglas no.

## Checks obligatorios

| Check | Backend | Frontend | Regla |
| --- | --- | --- | --- |
| Instalación reproducible | ✅ | ✅ | Con el lockfile congelado (`npm ci`, `pnpm install --frozen-lockfile`…). Si el lockfile no coincide con el manifiesto, falla |
| Lint | ✅ | ✅ | **Cero errores y cero advertencias** (`--max-warnings 0` o su equivalente) |
| Build de producción | ✅ | ✅ | Compila y valida tipos igual que en el despliegue |
| Pruebas unitarias con cobertura | ✅ ≥ 70 % | ✅ ≥ 70 % | El job falla si la cobertura de líneas baja del mínimo |
| Pruebas de integración (BD real) | ✅ ≥ 40 % | — | Contra una base de datos temporal levantada en el mismo job |
| Pruebas E2E por HTTP | ✅ ≥ 15 % | — | La app completa con su BD; flujo por actor/rol |
| E2E con Playwright | — | ✅ | Pocas, solo flujos críticos, con la API simulada. No se miden en porcentaje (ver [Qué probar](../02-Testing/que-probar-back-y-front.md)) |
| Auditoría de dependencias | ✅ | ✅ | Las **de producción**, sin vulnerabilidades **altas o críticas**. Las de desarrollo no llegan al servidor ni al navegador: las sube Dependabot |
| SonarQube Cloud | ✅ | — | Quality Gate en verde sobre `main` (ver abajo) |
| Simulacro de promoción | ✅ | ✅ | Ver [Despliegue y CI/CD](despliegue-y-ambientes.md) |

Los porcentajes son de **líneas cubiertas**. Cada nivel de prueba tiene su propio mínimo y su propio reporte (`coverage/unit`, `coverage/integration`, `coverage/e2e`).

**Repos nuevos:** cumplen los mínimos desde el primer PR. **Repos existentes sin pruebas:** el CI exige que la cobertura **no baje** y su página de la wiki fija las etapas para llegar al mínimo (por ejemplo 30 % → 50 % → 70 %). No se aceptan pruebas vacías para inflar el número.

## Qué cambia según la tecnología

La estructura del workflow es la misma en todos los repos: **instalar → auditoría → lint → pruebas con cobertura → build**. Lo que cambia es el comando de cada paso:

| Paso | NestJS / Node (pnpm) | React o Vue + Vite (npm) | Python |
| --- | --- | --- | --- |
| Instalar | `pnpm install --frozen-lockfile` | `npm ci` | `pip install -r requirements.txt` (versiones fijadas) |
| Lint | `pnpm lint` | `npm run lint` | `ruff check .` |
| Unitarias + cobertura | `pnpm test:cov` | `npm run test:cov` | `pytest --cov --cov-fail-under=70` |
| Integración / E2E | `pnpm test:int` · `pnpm test:e2e` | `npx playwright test` | `pytest tests/integration` |
| Build | `pnpm build` | `npm run build` | — (o el empaquetado que aplique) |
| Auditoría | `pnpm audit --prod --audit-level high` | `npm audit --omit=dev --audit-level=high` | `pip-audit` |

**Workflows de referencia**, listos para copiar y cambiar solo los comandos:
- Backend: `indigo-backend/.github/workflows/ci.yml` (con Postgres como servicio del job y SonarQube Cloud).
- Frontend: `indigo-user-space/.github/workflows/ci.yml`.
- Simulacro de promoción: `.github/workflows/promocion.yml` en los dos.

Si un repo usa otra tecnología, su página de [Stacks](../04-Stacks/README.md) dice cuál es el comando de cada paso. **Lo que no se puede es saltarse un paso.**

**Si la auditoría falla:**
1. Si la vulnerabilidad está en una dependencia directa, se sube a la versión de parche corregida (en un PR `chore:` aparte).
2. Si viene de una dependencia de otra (transitiva) y la directa todavía no la corrige, se fuerza la versión corregida con *overrides* (`overrides` en `pnpm-workspace.yaml`, o `overrides` en `package.json` con npm), con un comentario que diga cuándo se quita.
3. Nunca se baja el nivel de la auditoría ni se apaga el paso para que pase el CI.

## SonarQube Cloud (solo backends)

Analiza bugs, vulnerabilidades, *security hotspots*, código duplicado y la cobertura del código nuevo. Se usa **solo en los backends** porque el plan gratuito con repos privados tiene dos límites:

- **Tope de 50.000 líneas para toda la organización** `innlab2016`, no por repo. Reservarlo para los backends permite tener varios proyectos dentro del tope.
- **Solo analiza la rama principal**, no los PR. Por eso el análisis corre cuando los cambios llegan a `main`, no antes del merge.

Reglas:
- Si el Quality Gate falla sobre `main`, se abre un `fix/` **ese mismo día**, antes de cualquier otro PR del repo.
- Si se pasa a un plan pago o los repos se vuelven públicos, el análisis se mueve a los PR y bloquea el merge.

**Activarlo en un backend (una vez):**
1. En [sonarcloud.io](https://sonarcloud.io), con la cuenta de GitHub de la organización, importar el repo. La clave del proyecto es `innlab2016_<repo>` y va en `sonar-project.properties`.
2. En el proyecto: *Administration › Analysis Method* → apagar *Automatic Analysis* (el análisis lo hace el CI, con la cobertura).
3. Crear un token en *My Account › Security* y guardarlo como secreto del repo `SONAR_TOKEN` (*Settings › Secrets and variables › Actions*).

## Sin protección de ramas (situación actual)

Con el plan gratuito de GitHub y repos privados no se pueden proteger ramas: **el CI muestra ✅ o ❌ en el PR, pero no impide el merge.** Mientras sea así, la protección es una regla de equipo:

- **Nadie hace push directo** a `dev` ni a `main`.
- **Nadie fusiona su propio PR.**
- **Quien revisa comprueba que todos los checks estén en verde** antes de aprobar. Un PR con un check en rojo no se fusiona, sin excepción.

Cuando el plan lo permita, estas reglas se configuran en *Settings › Branches* y dejan de depender de la revisión.

## Versiones fijadas

Ninguna dependencia, herramienta o imagen se instala como "la última": todo va con su versión exacta y se actualiza por PR. El detalle está en [Dependencias y herramientas](dependencias-y-herramientas.md#versiones-fijadas).

## Ver también

[Despliegue y CI/CD](despliegue-y-ambientes.md) · [Issues y Pull Requests](issues-y-pull-requests.md) · [Qué probar en backend y frontend](../02-Testing/que-probar-back-y-front.md) · [Convenciones de testing](../02-Testing/convenciones.md)
