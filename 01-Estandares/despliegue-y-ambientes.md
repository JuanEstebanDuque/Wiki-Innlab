# Despliegue y CI/CD

📍 [InnLab](../README.md) › [Estándares](README.md) › **Despliegue y CI/CD**

> **Resumen:** **(En definición)** La plataforma de despliegue, la herramienta de CI y la infraestructura de base de datos (servidores de la universidad) se están definiendo. Esta card fija los **principios** que aplican sin importar lo que se elija. Opciones en [Pendientes por concretar](../pendientes-por-concretar.md).

## Situación actual

- La forma en que están desplegados hoy algunos servicios se va a reemplazar por un estándar nuevo, así que **no se documenta como referencia**.
- Varios repos todavía no se despliegan (Índigo, por ejemplo), pero lo harán pronto: deben prepararse con los principios de abajo.
- Ningún repo tiene CI/CD todavía.

## Principios (aplican ya, con cualquier plataforma)

1. **Nada hardcodeado por ambiente.** URLs de APIs, orígenes CORS, puertos y credenciales salen de variables de entorno.
2. **Los secretos nunca van en el repo**, ni siquiera en archivos de configuración de despliegue: se cargan en la plataforma o en su gestor de secretos.
3. **Configuración como código:** lo necesario para construir y correr el servicio (por ejemplo un `Dockerfile`) vive versionado en el repo, no solo en un panel web.
4. **Se despliega desde ramas, no desde máquinas personales:** `dev` va a staging y `main` va a producción.
5. **Cada servicio expone un health check** (`GET /health`), como ya hace [product-integration-guide](../03-InnLab-Core/product-integration-guide.md).
6. **Cada repo documenta en su página de la wiki** cómo se construye, qué variables necesita y dónde corre.

## Ambientes

| Ambiente | Rama | Para qué |
| --- | --- | --- |
| Local | cualquiera | Desarrollo, con `.env` propio |
| Staging | `dev` | Probar integrado antes del release |
| Producción | `main` | Usuarios reales |

## Pipeline de CI/CD (independiente de la herramienta)

Sea GitHub Actions, Jenkins u otra herramienta, el pipeline de cada repo tiene estas etapas:

| Etapa | Cuándo | Qué hace | Si falla |
| --- | --- | --- | --- |
| 1. Verificación | En cada PR | Instalar, lint, `test`, build | No se puede hacer merge |
| 2. Despliegue a staging | Merge a `dev` | Construir y desplegar a staging | Se avisa al autor |
| 3. Despliegue a producción | Merge a `main` (release) | Construir y desplegar a producción, con aprobación manual | Rollback a la versión anterior |

Para que la etapa 1 funcione con cualquier herramienta, todo repo expone los scripts estándar: `lint`, `test` y `build` (ver [Testing → Convenciones](../02-Testing/convenciones.md)).
