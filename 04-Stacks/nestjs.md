# NestJS (backend)

📍 [InnLab](../README.md) › [Stacks](README.md) › **NestJS**

> **Resumen:** TypeScript, un módulo por dominio, controller delgado, service con la lógica y DTOs validados. La configuración se valida al arrancar, la autenticación va con el guard global de InnLab Core y el prefijo de la API es `/api`.

**Repos:** [innlab-core-api](../03-InnLab-Core/innlab-core-api.md) · [product-integration-guide](../03-InnLab-Core/product-integration-guide.md) · [foresight-backend](../05-Proyectos/foresight/foresight-backend.md) · [indigo-backend](../05-Proyectos/indigo/indigo-backend.md)

## Arrancar un backend nuevo

Se parte de **[product-integration-guide](../03-InnLab-Core/product-integration-guide.md)**, que ya trae la autenticación con Core, el cliente de la API interna, la configuración validada y el health check. Ver [Integrar un producto](../03-InnLab-Core/integrar-un-producto.md).

## Estructura de carpetas

Base común del backend: [Estructura de carpetas](../01-Estandares/estructura-de-carpetas.md). Cómo adoptarla en repos existentes sin reestructurar todo de golpe también está ahí.


```
src/
  main.ts
  app.module.ts
  config/                → lectura y validación de las variables de entorno
  common/                → lo que usan varios módulos (según lo que haga falta:
                           decorators/, filters/, guards/, interceptors/, middleware/…)
  auth/                  → autenticación con InnLab Core (de product-integration-guide)
  <modulo>/              → un módulo por dominio, directo en src/ (ej. diagnosticador/)
    <modulo>.module.ts
    <modulo>.controller.ts
    <modulo>.service.ts
    dto/                 → si el módulo recibe datos
    entities/            → si el módulo tiene tablas en la BD
    interfaces/          → si el módulo tiene tipos propios
    …                    → otras carpetas que el módulo necesite (ej. search/, generate/, database/)
```

El mínimo es `config/`, `common/`, `auth/` y los módulos; dentro de cada módulo y de `common/` se crean solo las carpetas que se usen. Ejemplo real: `indigo-backend`, cuyo módulo `diagnosticador/` agrega `search/`, `generate/` y `database/`.

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

1. `nest g resource <dominio>` (queda directo en `src/<dominio>/`).
2. Entidad en `entities/` y migración.
3. DTOs con validación.
4. Tests del service en el mismo PR (ver [Testing](../02-Testing/README.md)).
5. Documentación en Swagger.
6. Variables nuevas en `.env.example` y en la validación de config.

## Tests

**Qué probar:** [Testing → Backend](../02-Testing/que-probar-back-y-front.md).

Framework libre por repo (hoy Jest o Vitest), con las [convenciones comunes](../02-Testing/convenciones.md): `*.spec.ts` junto al archivo, e2e en `test/`, y los scripts `test`, `test:watch`, `test:cov` y `test:e2e`.
