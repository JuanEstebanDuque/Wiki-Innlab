# Glosario

📍 [InnLab](../README.md) › Referencia › **Glosario**

> **Resumen:** términos de negocio y técnicos que aparecen en el código y en la wiki. Si un término de dominio queda en español dentro del código, tiene que estar aquí.

| Término | Significado | Dónde aparece |
| --- | --- | --- |
| **InnLab** | _[Completar: el área o laboratorio]_ | Todo |
| **InnLab Core** | Plataforma central: identidad, empresas y hub de productos | innlab-core-* |
| **Hub** | Pantalla de Core donde el usuario ve los productos a los que tiene acceso | innlab-core-user-space |
| **Producto** | Cada aplicación de InnLab accesible desde el hub (Foresight, Inndex…) | Core: `Product`, `CompanyProduct` |
| **SSO** | Single Sign-On: entrar a un producto sin volver a autenticarse, con un código de un solo uso que emite Core | Core, Foresight |
| **Cognito** | Servicio de identidad de AWS que usa Core. Reemplazó a Keycloak | Core, Foresight |
| **Key interna** | Credencial que identifica al backend de un producto ante Core (header `x-internal-key`). Una por producto; nunca en un frontend | Core, productos |
| **Slug de producto** | Identificador corto del producto en la tabla `Product` de Core (por ejemplo `foresight`), que se usa en `?product=` | Core |
| **Ucorp, diag-irl** | Productos con backend ya registrado en Core (keys reservadas), aún no documentados en esta wiki | Core |
| **product-integration-guide** | Plantilla NestJS con la integración con Core lista para copiar | Core |
| **Workspace** | Espacio de trabajo dentro de una empresa, con miembros y features | Core |
| **Feature** | Funcionalidad que se puede habilitar por usuario o por workspace | Core |
| **Vertical** | Sector que analiza Foresight (fintech, agrotech, biotech…) | Foresight |
| **Radar / Informe** | Entregable de Foresight con los hallazgos de una vertical | Foresight |
| **Custom made (a la medida)** | Servicio o informe hecho a pedido del cliente | Foresight |
| **Diagnóstico 1-3-9-45** | Estructura de Índigo: 1 diagnóstico → 3 macro categorías → 9 categorías → 45 ítems | Índigo |
| **Macro categoría / Categoría / Ítem** | Niveles 1, 2 y 3 del diagnóstico | Índigo (`macro_categorias`, `categorias`, `items`) |
| **Likert-6** | Escala de respuesta de 6 puntos, sin punto medio | Índigo |
| **Tier de literatura** | Indicador de calidad de un paper (citas, FWCI, año) | Índigo |
| **Aplicación** | Un diagnóstico aplicado a una empresa en un periodo | Índigo |
| **Inndex / Growinn Index** | Producto de encuestas de diagnóstico por áreas y jerarquías | Growinn |
| **Jerarquía** | Nivel organizacional: operativo, táctico o estratégico | Growinn, Índigo |
| **ADR** | Architecture Decision Record: documento corto que registra una decisión técnica | Documentación |
