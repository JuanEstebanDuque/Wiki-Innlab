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

## End-to-end (más adelante)

Cuando exista CI, se agregan **pocos** e2e, solo de los flujos críticos que cruzan front y back. Por ejemplo: hub de Core → SSO → entrar al producto → acción principal (crear un diagnóstico, ver un informe). No se usan para cubrir lo que ya cubren las pruebas unitarias.

## Ver también

[Principios](README.md) · [Qué y cómo testear](que-y-como-testear.md) · [Convenciones de nombres y scripts](convenciones.md)
