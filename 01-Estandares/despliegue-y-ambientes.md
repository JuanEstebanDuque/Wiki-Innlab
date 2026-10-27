# Despliegue y CI/CD

📍 [InnLab](../README.md) › [Estándares](README.md) › **Despliegue y CI/CD**

> **Resumen:** el CI corre en **GitHub Actions** (decidido) y sus checks están en [CI y calidad](ci-y-calidad.md). La **plataforma de despliegue** sigue **(En definición)**, pero el flujo ya está acordado: cada PR se despliega en **staging**, una persona lo valida **a mano** y, al aprobarlo y fusionarlo, los cambios pasan **solos a producción**. El rollback existe **solo en producción**. Mientras no haya plataforma, un **simulacro de promoción** verifica en cada merge que el paso a producción no se rompería.

## Principios (aplican ya, con cualquier plataforma)

1. **Nada hardcodeado por ambiente.** URLs de APIs, orígenes CORS, puertos y credenciales salen de variables de entorno.
2. **Los secretos nunca van en el repo**, ni siquiera en archivos de configuración de despliegue: se cargan en la plataforma o en su gestor de secretos.
3. **Configuración como código:** lo necesario para construir y correr el servicio (por ejemplo un `Dockerfile`) vive versionado en el repo, no solo en un panel web.
4. **Se despliega desde GitHub Actions, nunca desde máquinas personales.**
5. **Cada servicio expone un health check** (`GET /health`), como ya hace [product-integration-guide](../03-InnLab-Core/product-integration-guide.md).
6. **Cada repo documenta en su página de la wiki** cómo se construye, qué variables necesita y dónde corre.
7. **Las migraciones de BD son compatibles hacia atrás**: primero se agrega lo nuevo y lo viejo se quita en un despliegue posterior. Así el rollback del código no choca con la base de datos.

## Ambientes

| Ambiente | Qué tiene | Para qué |
| --- | --- | --- |
| Local | Cualquier rama, con `.env` propio | Desarrollo |
| Staging | **La rama del PR que se está revisando** | Que quien revisa pruebe el cambio a mano, integrado, antes de aprobarlo |
| Producción | `main` | Usuarios reales |

**Un PR a la vez en staging.** Si hay dos PR abiertos, se valida uno, se fusiona, y después se despliega y valida el otro.

## Flujo de un cambio (acordado)

```
PR abierto ─▶ CI (checks) ─▶ despliegue a STAGING + smoke test
                                    │
                                    ▼
                  Validación MANUAL en staging (quien revisa)
                                    │ aprueba y fusiona
                                    ▼
        AUTOMÁTICO: dev → main ─▶ despliegue a PRODUCCIÓN + smoke test + tag vX.Y.Z
                                    │ si algo falla
                                    ▼
                  ROLLBACK manual, solo en producción
```

| Paso | Cuándo | Quién | Qué pasa | Si falla |
| --- | --- | --- | --- | --- |
| 1. Checks | Al abrir el PR y en cada push | Automático | Los de [CI y calidad](ci-y-calidad.md) | El PR no se fusiona |
| 2. Despliegue a staging | Al abrir el PR | Automático | Se despliega la rama del PR y corre el **smoke test** | Se avisa al autor |
| 3. Validación en staging | Después del paso 2 | **Quien revisa (manual)** | Prueba en staging **todo lo que dice "Qué cambia"** del PR y deja constancia en el PR | Pide cambios; no se aprueba |
| 4. Merge | Cuando el paso 3 está bien | Quien revisa | Aprueba y fusiona a `dev` | — |
| 5. Promoción a producción | Al fusionar | Automático | `dev` → `main` (merge commit), despliegue a producción, smoke test y tag de la versión | Se avisa y se decide el rollback |
| 6. Rollback | Solo si producción falla | Manual (botón del workflow) | Vuelve a desplegar en producción un tag anterior. **Staging no se toca** | — |

**El smoke test** es una prueba corta contra el ambiente desplegado: en el backend, `GET /health` y `GET /health/ready` dan 200 y una ruta protegida da 401 sin token; en el frontend, la app carga sin errores en consola, las rutas de la SPA responden y el backend contesta. Vive en `.github/workflows/smoke.yml` de cada repo y corre desde GitHub Actions, sin servidor propio. Mientras no haya plataforma, se lanza a mano con la URL del ambiente.

## Mientras no hay plataforma: simulacro de promoción

Cada repo tiene `.github/workflows/promocion.yml`. Cada vez que algo entra a `dev`:
1. Une `dev` con `main` **dentro del runner** (no publica nada ni toca `main`).
2. Corre lint, build y todas las pruebas sobre ese resultado.

Si falla, la promoción real fallaría igual: por ejemplo, porque alguien fusionó algo directo a `main` que choca con `dev`. Se arregla antes de seguir.

Cuando se defina la plataforma, el simulacro se reemplaza por los pasos 2, 5 y 6 reales, y esta página se actualiza con el ADR correspondiente (ver [Pendientes](../pendientes-por-concretar.md)).

## Ver también

[CI y calidad](ci-y-calidad.md) · [Issues y Pull Requests](issues-y-pull-requests.md) · [Versionado y releases](versionado-y-releases.md)
