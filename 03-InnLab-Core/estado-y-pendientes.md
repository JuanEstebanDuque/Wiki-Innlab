# Estado y pendientes de Core

📍 [InnLab](../README.md) › [InnLab Core](README.md) › **Estado y pendientes**

> **Resumen:** lo que Core **todavía no ofrece**. Si tu producto lo necesita, coordínalo con el equipo de Core en vez de implementarlo por tu cuenta en el producto.

| Pendiente | Situación actual | Qué hacer mientras tanto |
| --- | --- | --- |
| **Catálogo de productos** | No existe `GET /hub/products`: los productos del hub están hardcodeados en el frontend (`PRODUCT_CONFIG`) | Pedir al equipo de Core que agregue tu producto |
| **Asignación de roles** | `POST /internal/roles/assign` responde, pero no guarda nada | No depender de él |
| **Roles por empresa en el token** | El JWT no trae `companyRole`, así que `RolesGuard` no lo tiene | Llamar a `getUserContext(user.id)` en un interceptor y agregar `companyRole` a `req.user` **antes** del `RolesGuard`. Ese interceptor aún no existe en ningún producto: si lo construyes, se sube a [product-integration-guide](product-integration-guide.md) |
| **Workspaces** | Las entidades existen, pero no hay módulo activo | No usarlos todavía |
| **Feature flags** | Solo lectura: no hay endpoint para crearlos ni asignarlos | Se cargan a mano en la BD de Core |
| **Panel de administración** | No hay UI para gestionar miembros ni invitaciones | — |
| **Registro de productos y keys** | Manual: tabla `Product` y `internal.guard.ts` | Pedirlo al equipo de Core |
