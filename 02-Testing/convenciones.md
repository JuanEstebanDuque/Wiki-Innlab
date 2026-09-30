# Convenciones de testing

📍 [InnLab](../README.md) › [Testing](README.md) › **Convenciones**

> **Resumen:** cada repo puede usar el framework que quiera, pero desde fuera todos se ven igual: los tests van junto al archivo, se llaman igual y se corren con los mismos scripts.

## Nombres y ubicación

| Tipo de repo | Test unitario | Test de integración / e2e |
| --- | --- | --- |
| Backend (TS/JS) | `nombre.spec.ts`, junto al archivo | `test/<flujo>.e2e-spec.ts` |
| Frontend (React / Vue) | `Componente.test.tsx` o `Componente.test.ts`, junto al componente | `e2e/<flujo>.spec.ts` |
| Python | `tests/test_<modulo>.py` | `tests/integration/test_<flujo>.py` |

Los mocks compartidos van en `test/mocks/` (o en `tests/fixtures/` en Python).

## Scripts comunes

Todo repo expone **los mismos nombres de script**, sin importar el framework que tenga debajo. Así, una persona nueva y el CI saben cómo correrlos sin leer la configuración.

| Script | Qué hace |
| --- | --- |
| `test` | Corre todos los tests unitarios una vez |
| `test:watch` | Modo watch para desarrollo |
| `test:cov` | Tests con reporte de cobertura |
| `test:e2e` | Tests de integración / e2e (si existen) |

En Python, el equivalente es `pytest`, `pytest --cov` y `pytest tests/integration`, documentados en el README.

## Framework

**Libre por repo.** Lo que se pide es que:
- Se documente en el README cuál se usa.
- Exponga los scripts de arriba.
- Genere un reporte de cobertura que el CI pueda leer, cuando exista.

Hoy se usan Jest (innlab-core-api, foresight-backend, product-integration-guide), Vitest (indigo-backend) y pytest (propuesto para scrapping).

## Cobertura

- **No se exige cobertura retroactiva.** La regla es que **el código nuevo con lógica entra con test** y que la cobertura del repo **no baja**.
- **(Propuesta)** Cuando exista CI, un PR que baje la cobertura del repo no se puede mergear.
- No se persigue un porcentaje por sí mismo: 100 % de cobertura con tests que no verifican nada no sirve.

## Estado actual por repo

| Repo | Framework | Tests reales | Prioridad para empezar |
| --- | --- | --- | --- |
| [indigo-backend](../05-Proyectos/indigo/indigo-backend.md) | Vitest | ✅ Un `.spec.ts` por archivo del pipeline. **Repo de referencia** | — |
| [innlab-core-api](../03-InnLab-Core/innlab-core-api.md) | Jest | ❌ Solo el de la plantilla | Guards (Cognito, interno) y SSO |
| [product-integration-guide](../03-InnLab-Core/product-integration-guide.md) | Jest | ❌ Solo el e2e de la plantilla | `/health` 200 y `/api/me/context` 401 |
| [foresight-backend](../05-Proyectos/foresight/foresight-backend.md) | Jest | ❌ Ninguno | `reports`, `subscriptions`, cliente de Core |
| [innlab-core-user-space](../03-InnLab-Core/innlab-core-user-space.md) | — | ❌ | `AuthContext`, `SsoPage` |
| [foresight-user-space](../05-Proyectos/foresight/foresight-user-space.md) | — | ❌ | `services/`, recepción del SSO |
| [foresight-landing](../05-Proyectos/foresight/foresight-landing.md) | — | ❌ | Baja (contenido estático) |
| [indigo-user-space](../05-Proyectos/indigo/indigo-user-space.md) | — | ❌ (recién creado) | Composables, `services/` y recepción del SSO |
| [foresight-scrapping](../05-Proyectos/foresight/foresight-scrapping.md) | — | ❌ | Funciones puras al extraerlas a `src/` |
| [Growinn](../05-Proyectos/growinn-index/growinn-index-test.md) | — | ❌ | No se invierte (se reemplaza por Índigo) |
