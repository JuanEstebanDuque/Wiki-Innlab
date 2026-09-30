# Express / Node (legado)

📍 [InnLab](../README.md) › [Stacks](README.md) › **Express / Node**

> **Resumen:** solo se usa en el backend de Growinn Index, que **será reemplazado por Índigo**. No se inician proyectos nuevos con Express suelto: se usa [NestJS](nestjs.md). Mientras Growinn exista, se mantiene tal cual y se siguen estas reglas mínimas en lo que se toque.

**Repos:** [Growinn Index — Backend](../05-Proyectos/growinn-index/growinn-index-test.md)

## Reglas mínimas mientras siga vivo

- **Separar rutas de lógica:** `routes/` solo define endpoints y llama funciones de `services/`. La lógica (cálculo de puntajes, correos, Excel) no vive dentro del handler.
- **Configuración desde `.env`**, validada al arrancar, y con `.env.example` al día (hoy no coincide con el código).
- **CORS desde variable de entorno**, con un solo nombre de variable, no varios (`ALLOWED_ORIGINS`, `ADDITIONAL_ORIGINS`, `FRONTEND_ORIGIN`…).
- **Middleware de errores** único al final de la cadena. Nada de `res.status(500).send(err)` repetido en cada ruta.
- **Sin dependencias basura:** se quitan `fs`, `path` y `https` de `package.json` (son módulos nativos).
- **HTTPS local con certificados propios solo en desarrollo.** En los ambientes desplegados, el TLS lo pone la plataforma.
- **Tests:** si se toca lógica de cálculo (`constants/calculateScore.js`, `categorizeAnswer.js`), se agregan tests.

## Futuro

No se migra a otro stack: el producto será reemplazado por [Índigo](../05-Proyectos/indigo/README.md). Su lógica de puntajes (`constants/`) sirve como referencia para esa migración.
