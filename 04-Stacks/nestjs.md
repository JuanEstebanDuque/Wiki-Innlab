# NestJS (backend)

📍 [InnLab](../README.md) › [Stacks](README.md) › **NestJS**

> **Resumen:** TypeScript, un módulo por dominio, controller delgado, service con la lógica y DTOs validados. La configuración se valida al arrancar, la autenticación va con el guard global de InnLab Core y el prefijo de la API es `/api`.

**Repos:** [innlab-core-api](../03-InnLab-Core/innlab-core-api.md) · [product-integration-guide](../03-InnLab-Core/product-integration-guide.md) · [foresight-backend](../05-Proyectos/foresight/foresight-backend.md) · [indigo-backend](../05-Proyectos/indigo/indigo-backend.md)

## Arrancar un backend nuevo

Se parte de **[product-integration-guide](../03-InnLab-Core/product-integration-guide.md)**, que ya trae la autenticación con Core, el cliente de la API interna, la configuración validada y el health check. Ver [Integrar un producto](../03-InnLab-Core/integrar-un-producto.md).

## Estructura de carpetas

```
src/
  main.ts                 → bootstrap: prefijo /api, ValidationPipe, CORS desde env
  app.module.ts
  config/namespaces/      → configuración por namespace, validada al arrancar
  auth/                   → guard JWT global + decoradores (viene de la plantilla)
  innlab-api/             → cliente de la API interna de Core (viene de la plantilla)
  health/                 → GET /health
  common/                 → filtros, interceptors y utilidades compartidas
  modules/
    <dominio>/
      <dominio>.module.ts
      <dominio>.controller.ts
      <dominio>.service.ts
      <dominio>.service.spec.ts
      dto/                → create-<x>.dto.ts, update-<x>.dto.ts
      entities/           → <x>.entity.ts
test/                     → e2e
```

`indigo-backend` agrupa por etapa del pipeline (`search/`, `generate/`, `save/`) dentro de un módulo. Es válido **dentro** de un módulo.

## Reglas de código

- **Controller delgado:** recibe el DTO, llama al service y devuelve la respuesta. No tiene lógica de negocio ni acceso a datos.
- **El service concentra la lógica** y es el único que usa repositorios o clientes externos.
- **Inyección por constructor.** Nunca se hace `new` de un service.
- **Toda entrada se valida** (DTOs con `class-validator` o esquemas Zod) y hay un `ValidationPipe` global con `whitelist` y `forbidNonWhitelisted`. Ningún endpoint recibe `any`.
- **Las respuestas de un LLM o de una API externa se validan con un esquema** antes de usarse.
- **Configuración validada al arrancar:** si falta una variable, la app no levanta.
- **Autenticación:** guard JWT global de la plantilla; las rutas públicas se marcan con `@Public()`. No se implementa un login propio.
- **Base de datos:** cambios de esquema **solo por migraciones versionadas**; nada de sincronización automática del esquema ni SQL suelto en el README.
- **CORS desde variable de entorno**, nunca con una lista hardcodeada.
- **Errores:** excepciones HTTP de Nest (o propias que las extienden), capturadas en un filtro global. No hay `catch` vacíos.
- **Logs:** con el `Logger` de Nest, sin `console.log`. Nunca se loguean tokens, contraseñas ni datos personales.

## API REST

- Prefijo global **`/api`**, sin versión en la ruta.
- Recursos en plural y kebab-case: `/api/custom-made-assets`.
- Todo endpoint documentado en Swagger (`@ApiOperation`, `@ApiResponse`).

## Crear un módulo nuevo

1. `nest g resource modules/<dominio>`.
2. Entidad en `entities/` y migración.
3. DTOs con validación.
4. Tests del service en el mismo PR (ver [Testing](../02-Testing/README.md)).
5. Documentación en Swagger.
6. Variables nuevas en `.env.example` y en la validación de config.

## Tests

**Qué probar:** [Testing → Backend](../02-Testing/que-probar-back-y-front.md).

Framework libre por repo (hoy Jest o Vitest), con las [convenciones comunes](../02-Testing/convenciones.md): `*.spec.ts` junto al archivo, e2e en `test/`, y los scripts `test`, `test:watch`, `test:cov` y `test:e2e`.
