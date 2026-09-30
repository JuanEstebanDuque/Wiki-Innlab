# Proyectos

📍 [InnLab](../README.md) › **Proyectos**

Cada proyecto es un producto de InnLab que se construye **sobre [InnLab Core](../03-InnLab-Core/README.md)**. Su página explica qué es, cómo se conectan sus repos y qué reglas propias tiene. El detalle técnico está en la página de cada repo.

## Catálogo

| Proyecto | Qué es | Repos | Integración con Core | Estado |
| --- | --- | --- | --- | --- |
| [Foresight](foresight/README.md) | Vigilancia tecnológica: informes, radar y verticales | [backend](foresight/foresight-backend.md) · [user-space](foresight/foresight-user-space.md) · [landing](foresight/foresight-landing.md) · [scrapping](foresight/foresight-scrapping.md) | ✅ Integrado (es la referencia) | 🟢 Activo |
| [Índigo](indigo/README.md) | Diagnósticos organizacionales 1-3-9-45 generados con IA | [backend](indigo/indigo-backend.md) · [user-space](indigo/indigo-user-space.md) | ⏳ Pendiente (obligatorio) | 🟡 En desarrollo; se desplegará pronto |
| [Growinn Index](growinn-index/README.md) | Encuestas de diagnóstico por áreas y jerarquías | [monorepo](growinn-index/growinn-index-test.md) | ❌ Usa Supabase Auth | 🟠 Se mantiene tal cual; será reemplazado por Índigo |

Ucorp y diag-irl ya tienen key en Core, pero todavía no están documentados aquí.

## Plantilla de página de proyecto

1. **Descripción**: la escribe el líder del proyecto.
2. **Ficha**: estado, repos, stack, integración con Core, datos y responsable.
3. **Arquitectura**: cómo se conectan los repos entre sí y con InnLab Core.
4. **Reglas propias del proyecto**: solo lo que **agrega** a los [Estándares](../01-Estandares/README.md). Si no hay nada, se dice "ninguna".
5. **Repos**: una card por repo.

## Agregar un proyecto nuevo

1. Crear los repos en `innlab2016` con nombre `<proyecto>-<rol>` (`<proyecto>-backend`, `<proyecto>-user-space`, `<proyecto>-landing`).
2. **Backend: arrancar desde [product-integration-guide](../03-InnLab-Core/product-integration-guide.md).**
3. Seguir [Integrar un producto](../03-InnLab-Core/integrar-un-producto.md): slug, key interna, JWT y SSO.
4. Crear la página del proyecto y la de cada repo, copiando la estructura de uno existente.
5. Agregarlo a este catálogo y al [Mapa de servicios](../06-Referencia/mapa-de-servicios.md).
