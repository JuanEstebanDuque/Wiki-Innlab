# Qué y cómo testear

📍 [InnLab](../README.md) › [Testing](README.md) › **Qué y cómo testear**

> **Resumen:** muchos tests unitarios rápidos, algunos de integración y pocos end-to-end. Se prioriza lo que más duele si falla, cada test sigue la estructura Preparar → Ejecutar → Verificar, y todo lo externo se reemplaza por mocks.

## Tipos de test

| Tipo | Qué prueba | Cuánto | Ejemplo |
| --- | --- | --- | --- |
| **Unitario** | Una función, un service o un componente, aislado | La mayoría | `calculateScore()` devuelve el puntaje correcto para cada respuesta |
| **Integración** | Varias piezas juntas: endpoint + service + BD de prueba | Algunos | `POST /reports` guarda y devuelve el informe |
| **End-to-end** | Un flujo completo, como lo hace el usuario | Pocos, solo flujos críticos | Login por SSO → hub → entrar a Foresight |
| **Manual** | Lo visual o lo que aún no se automatiza | Documentado en el PR | Revisar el responsive de la landing |

## Qué testear primero

Donde hoy no hay tests se empieza por lo que más duele si falla:

1. **Autenticación y autorización**: validación del JWT, guards, recepción del SSO y uso de la key interna.
2. **Lógica de negocio con cálculos o reglas**: puntajes, rankings, generación de diagnósticos, acceso por plan.
3. **Transformación de datos externos**: parsers, mappers, validación de las respuestas de un LLM o de una API.
4. **Flujos de UI con lógica**: formularios, pasos de un wizard, manejo de errores.
5. **Al final**, los componentes puramente visuales.

**Qué no vale la pena testear:** código generado, getters triviales, la configuración de un framework o librerías de terceros.

## Estructura de un test: Preparar → Ejecutar → Verificar

```
describe('<unidad que se prueba>')
  it('<hace qué> cuando <condición>')
    // Preparar: datos y mocks
    // Ejecutar: una sola acción
    // Verificar: el resultado esperado
```

- El nombre describe el comportamiento: `returns 401 when token is missing`, no `test1`.
- **Una sola acción por test.** Si necesitas verificar dos comportamientos, son dos tests.
- Sin lógica dentro del test (ni `if` ni bucles): si el test necesita lógica, es difícil de leer.

## Mocks y datos de prueba

- **Todo servicio externo se mockea:** InnLab Core, Cognito, S3, APIs de IA, correo y scraping.
- Los mocks que se repiten van en un helper compartido (`test/mocks/`), no se copian en cada archivo.
- Los datos de prueba son **ficticios**: ni correos reales, ni nombres de clientes, ni datos de producción.
- Para BD en tests de integración se usa una base de prueba desechable, nunca la de desarrollo compartida.
- **Tokens de prueba:** para testear la validación de JWT se generan tokens firmados con una clave de prueba. Nunca se usa un token real de Cognito.

## Tests que fallan a veces (flaky)

Un test que a veces pasa y a veces no **se arregla o se desactiva con un issue asociado** el mismo día. No se deja "re-correr hasta que pase".
