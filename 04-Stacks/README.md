# Stacks

📍 [InnLab](../README.md) › **Stacks**

Una card por cada tecnología que se usa **hoy** en InnLab. Aquí está **cómo se aplican los [Estándares](../01-Estandares/README.md) en cada stack**: estructura de carpetas, cómo crear un módulo y patrones a seguir.

- **Las stacks no imponen herramientas.** Describen patrones de código y organización. El gestor de paquetes o el framework de tests los elige cada repo (ver [Dependencias](../01-Estandares/dependencias-y-herramientas.md) y [Testing](../02-Testing/README.md)).
- Las páginas de repo **no repiten** esto: enlazan su stack y solo documentan lo que tienen de particular.

| Stack | Estado | Repos |
| --- | --- | --- |
| [NestJS (backend)](nestjs.md) | ✅ En uso para backends | innlab-core-api, product-integration-guide, foresight-backend, indigo-backend |
| [React + Vite (frontend)](react-vite.md) | ✅ En uso para frontends | innlab-core-user-space, foresight-user-space, foresight-landing, Growinn Frontend |
| [Vue + Vite (frontend)](vue-vite.md) | ✅ En uso para frontends | indigo-user-space |
| [Express / Node](express-node.md) | ⚠️ Legado: solo Growinn, que será reemplazado por Índigo | Growinn Backend |
| [Python / notebooks](python-notebooks.md) | ✅ Para scraping y análisis de datos | foresight-scrapping |

**Un backend de producto nuevo arranca desde [product-integration-guide](../03-InnLab-Core/product-integration-guide.md)**, que ya trae la integración con InnLab Core.
