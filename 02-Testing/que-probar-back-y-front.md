# Qué probar en backend y frontend

📍 [InnLab](../README.md) › [Testing](README.md) › **Qué probar en backend y frontend**

> **Resumen:** **si el cambio tiene lógica, va con test en el mismo PR. Si es solo visual, basta la prueba manual documentada en el PR.** Aplica igual al back y al front, y sin atarse a un framework: sirve para NestJS, React y Vue.

## ¿Mi cambio necesita test?

| Tipo de cambio | ¿Test automático? | Si no, ¿qué se hace? |
| --- | --- | --- |
| Lógica nueva o modificada (reglas, cálculos, validaciones, transformación de datos) | ✅ Sí, obligatorio | — |
| Corrección de un bug | ✅ Sí: un test que falla sin el fix y pasa con él | — |
| Nuevo endpoint o cambio en uno existente | ✅ Sí | — |
| Autenticación, permisos, SSO | ✅ Sí, siempre | — |
| Cambio puramente visual (estilos, maquetado, textos, íconos) | ❌ No | Prueba manual con capturas en "Cómo se probó" |
| Configuración, dependencias, documentación | ❌ No | Que build, lint y los tests existentes sigan pasando |

## Backend

| Qué probar | Cómo | Ejemplo |
| --- | --- | --- |
| **Services (lógica de negocio)** | Unitaria. Repositorios, Core y APIs externas mockeados | `calculateScore` devuelve el puntaje correcto; `generateMacros` rechaza una respuesta del LLM inválida |
| **Guards y autorización** | Unitaria o integración | Sin token → 401; token inválido o `id_token` → 401; sin permiso → 403; ruta `@Public()` → 200 |
| **Validación de entrada** | Integración del endpoint | Body con campos faltantes o extra → 400 |
| **Endpoints críticos** | Integración: request → controller → service → BD de prueba | `POST /api/reports` guarda y devuelve el informe |
| **Clientes de servicios externos** (Core, APIs de IA, scraping) | Unitaria con respuestas simuladas | Si Core responde 500 o no responde, el error se maneja y no se cae la app |
| **Migraciones** | Que corren sobre una BD vacía | — |

**No se prueba en backend:** controllers que solo delegan (se cubren con la integración), DTOs sin lógica, módulos de configuración del framework.

## Frontend

| Qué probar | Cómo | Ejemplo |
| --- | --- | --- |
| **Composables / hooks** | Unitaria | `useDiagnostic` calcula el progreso; `useAuth` detecta la sesión vencida |
| **`services/`** (llamadas a APIs) | Unitaria con HTTP mockeado | Arma bien la petición con el `Authorization`; ante un 401 redirige al hub; ante un 500 devuelve un error manejable |
| **Formularios** | Componente | Validación de campos, mensajes de error, botón deshabilitado mientras envía |
| **Flujos de varios pasos** (wizards) | Componente | No se puede avanzar sin completar el paso; volver atrás conserva los datos |
| **Renderizado condicional** | Componente | Estado vacío, cargando y error; lo que ve cada rol |
| **Recepción del SSO y guardas de ruta** | Unitaria o componente | Con `?code=` válido guarda el `accessToken`; sin sesión redirige al hub de Core |

**Cómo probar componentes:** desde el punto de vista del usuario (lo que se ve en pantalla y lo que pasa al hacer clic o escribir), no la implementación interna (estado privado ni nombres de funciones).

**No se prueba en frontend con tests automáticos:** componentes puramente visuales, estilos, maquetado ni animaciones. Van como **prueba manual en el PR**, con capturas de escritorio y móvil si aplica.

## Pruebas obligatorias por tipo de repo

Esto es lo que **todo** backend y frontend debe tener, sea cual sea su tecnología. Los mínimos de cobertura y cómo se corren en el CI están en [CI y calidad](../01-Estandares/ci-y-calidad.md).

### Backend

| Nivel | Qué prueba | Mínimo de líneas cubiertas |
| --- | --- | --- |
| **Unitarias** | Cada service, guard, cliente externo y validación, aislado | 70 % |
| **Integración** | Los services reales contra una **BD real temporal** (migraciones + seed); solo se simulan IA, Core y otros externos | 40 % |
| **E2E por HTTP** | La app completa por HTTP, con tokens firmados por un Core simulado y la BD real, **un flujo por actor o rol** | 15 % |
| **SonarQube Cloud** | Bugs, vulnerabilidades y código duplicado sobre `main` | Quality Gate en verde |
| **Manual en staging** | Lo que dice el PR, probado por quien revisa | — |

### Frontend

| Nivel | Qué prueba | Mínimo |
| --- | --- | --- |
| **Unitarias y de componentes** | Composables/hooks, `services/`, formularios, wizards, guardas de ruta y renderizado por rol (tabla de arriba) | 70 % de líneas |
| **E2E con Playwright** | **Pocos** flujos críticos de punta a punta en el navegador, con el build de producción y la **API simulada** (no depende de un backend corriendo). Por ejemplo: recepción del SSO → panel principal → acción principal del producto | Sin porcentaje: un test por flujo crítico |
| **Smoke test con Playwright** | Después de cada despliegue (staging y producción): la app carga, no hay errores en consola y el `GET /health` del backend responde | Debe pasar; si falla en producción se evalúa el rollback |
| **Manual** | Todo lo visual (estilos, maquetado, responsive) y lo que dice el PR, probado en staging por quien revisa | Capturas de escritorio y móvil en el PR |

**Por qué Playwright no se mide en porcentaje:** la cobertura de líneas la dan las pruebas unitarias. Las E2E son pocas y más lentas, pero cada una recorre un flujo completo (pantalla + rutas + API), que es justo lo que las unitarias no ven. Se reservan para lo que, si se rompe, deja al usuario sin poder trabajar.

**Dónde corren:** todo corre en GitHub Actions (Playwright usa un navegador sin interfaz dentro del runner). No se necesita un servidor propio.

## Ver también

[Principios](README.md) · [Qué y cómo testear](que-y-como-testear.md) · [Convenciones de nombres y scripts](convenciones.md)
