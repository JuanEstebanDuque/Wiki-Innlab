# InnLab Core

📍 [InnLab](../README.md) › **InnLab Core**

> _[Descripción de InnLab Core — la escribe su líder.]_
>
> **Resumen:** InnLab Core es la plataforma central del ecosistema. Da la **identidad** (login con AWS Cognito), las **empresas y sus miembros**, el **hub** donde el usuario ve sus productos y el **SSO** para entrar a cada producto sin volver a autenticarse. **Todos los productos de InnLab se autentican a través de Core.** No es un producto más: es la base sobre la que se construyen los demás.

## Reglas de oro

1. **Toda autenticación pasa por InnLab Core.** Ningún producto crea su propio login, su propio User Pool de Cognito ni su propia tabla de usuarios.
2. **Nunca se hace JOIN entre la base de un producto y la de Core.** Los IDs de usuario y de empresa son referencias lógicas: se guardan como texto y se consultan por la [API interna](api-interna.md).
3. **No se duplican datos de Core** (nombre de la empresa, rol, miembros). Se piden a Core cuando se necesitan.
4. **La API interna es solo entre backends.** Nunca se llama desde un navegador, y cada backend tiene su propia key.
5. **Un producto nuevo arranca desde [product-integration-guide](product-integration-guide.md)**, no copiando código de otro producto.

## Páginas de esta sección

| Página | Para qué |
| --- | --- |
| [Integrar un producto](integrar-un-producto.md) | Checklist paso a paso para conectar un producto nuevo (o migrar uno existente) |
| [Autenticación y SSO](autenticacion-y-sso.md) | Cómo llega el usuario desde el hub, cómo se valida el token y cómo se cierra la sesión |
| [API interna](api-interna.md) | Endpoints `/internal/*`, la key por backend y los endpoints públicos de auth |
| [Estado y pendientes de Core](estado-y-pendientes.md) | Lo que Core todavía no ofrece, para no depender de ello |

## Productos conectados

| Producto | Backend identificado como | Estado de integración |
| --- | --- | --- |
| [Foresight](../05-Proyectos/foresight/README.md) | `foresight-service` | ✅ En producción. Es la integración de referencia |
| Inndex / [Growinn Index](../05-Proyectos/growinn-index/README.md) | `inndex-service` | 🔑 Key reservada, sin integrar (el producto se reemplazará por Índigo) |
| Ucorp | `ucorp-service` | 🔑 Key reservada. _Proyecto aún no documentado en esta wiki_ |
| diag-irl | `diag-irl-service` | 🔑 Key reservada. _Proyecto aún no documentado en esta wiki_ |
| [Índigo](../05-Proyectos/indigo/README.md) | _por asignar_ | ⏳ Debe integrarse |

## Repos

| Repo | Qué es |
| --- | --- |
| [innlab-core-api](innlab-core-api.md) | API de Core: auth, SSO, onboarding y API interna |
| [innlab-core-user-space](innlab-core-user-space.md) | Frontend: landing, login y registro, onboarding y hub |
| [product-integration-guide](product-integration-guide.md) | **Plantilla NestJS** con la capa de integración lista para copiar |

## Ficha

| | |
| --- | --- |
| Stack | NestJS + React TS |
| Identidad | AWS Cognito (Core es el dueño del User Pool) |
| Datos | Base propia de Core |
| Responsable | _por definir_ |
