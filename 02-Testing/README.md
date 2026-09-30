# Testing

📍 [InnLab](../README.md) › **Testing**

> **Resumen:** los tests **viajan con el código**, en la misma rama y el mismo PR. Toda lógica nueva o modificada entra con su test. El estándar define **cómo se ven y se ejecutan** los tests, no qué framework usar: cada repo elige el suyo (Jest, Vitest, pytest…).

## Principios

1. **El test va en el mismo PR que el código que prueba**, tanto en backend como en frontend (ver [qué probar en cada uno](que-probar-back-y-front.md)). No hay rama `test/` ni "PR de tests después". Un PR con lógica nueva y sin tests se devuelve, salvo que justifique por qué no aplica.
2. **Se testea comportamiento, no implementación.** El test verifica qué hace la unidad, no cómo lo hace por dentro. Si refactorizas sin cambiar el comportamiento, los tests no deberían romperse.
3. **Cada unidad cubre el caso feliz y al menos un caso de error o de borde.**
4. **Los tests son independientes y repetibles**: no dependen del orden, de la hora, de la red ni de datos que dejó otro test.
5. **Nada real en los tests**: ni credenciales, ni datos de producción, ni llamadas a servicios externos (Cognito, InnLab Core, S3, APIs de IA…).
6. **Independientes de la tecnología:** da igual el framework, pero los nombres, la ubicación y los comandos son los mismos en todos los repos (ver [Convenciones](convenciones.md)).

## ¿Por qué no una rama `test`?

| Rama `test` separada | Tests en el mismo PR |
| --- | --- |
| Los tests quedan desincronizados del código que prueban | El test y el código cambian juntos |
| Se mergea código a `dev` sin probar | El revisor ve el test junto al cambio |
| Alguien tiene que acordarse de escribirlos después (y no pasa) | Forma parte de la [Definition of Done](../01-Estandares/definition-of-done.md) |

## Flujo

1. Escribes el código y su test en tu rama `feat/…` o `fix/…`.
2. Corres los tests en local antes de hacer push (`npm test` o su equivalente).
3. Abres el PR. En "Cómo se probó" dices qué tests agregaste.
4. **Cuando exista CI**, los tests corren solos en cada PR, y si fallan no se puede hacer merge (ver [Despliegue y CI/CD](../01-Estandares/despliegue-y-ambientes.md)).

## Páginas de esta sección

| Página | Qué resuelve |
| --- | --- |
| [Qué probar en backend y frontend](que-probar-back-y-front.md) | **Qué necesita test en cada capa**, y cuándo basta la prueba manual |
| [Qué y cómo testear](que-y-como-testear.md) | Tipos de test, qué priorizar, estructura de un test, mocks y datos de prueba |
| [Convenciones](convenciones.md) | Nombres de archivos, ubicación, scripts comunes, cobertura y estado por repo |
