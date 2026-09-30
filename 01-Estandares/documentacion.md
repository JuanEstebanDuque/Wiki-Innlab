# Documentación

📍 [InnLab](../README.md) › [Estándares](README.md) › **Documentación**

> **Resumen:** **la wiki (Notion) es la fuente de verdad.** Cada repo tiene un README corto para arrancar rápido, con enlace a su página en la wiki. Las decisiones importantes se registran como ADR.

## Qué va dónde

| Documento | Dónde vive | Contenido |
| --- | --- | --- |
| **Esta wiki** | Notion | Estándares, testing, InnLab Core, stacks, proyectos y el detalle de cada repo |
| `README.md` | Raíz de cada repo | **Corto:** qué es, cómo correrlo en 3-4 comandos y **enlace a su página en la wiki** |
| `.env.example` | Raíz de cada repo | Nombres de todas las variables de entorno |
| ADRs | `docs/adr/NNNN-titulo.md` | Decisiones de arquitectura |
| Colección de API | `postman/` o Swagger | Endpoints y cómo probarlos |

**No se duplica en el repo lo que está en la wiki.** No hay `CONTRIBUTING.md` largo ni guías copiadas: el README enlaza.

**`docs/` es solo para documentación del proyecto escrita por el equipo** (ADRs, diagramas, manuales). No es lugar para archivos generados por asistentes de IA.

## README corto

Plantilla en [Plantillas](../06-Referencia/plantillas.md). Tiene:

1. Qué es el repo (1-2 líneas) y a qué proyecto pertenece
2. **Enlace a su página en la wiki**, donde está todo el detalle
3. Arranque rápido: instalar, copiar `.env.example` y correr
4. Cómo correr tests

**Hoy no cumplen:** `innlab-core-api` e `innlab-core-user-space` (README de la plantilla), `foresight-backend` (describe Keycloak, pero se usa Cognito) y `foresight-scrapping` (sin README).

## Documentación dentro del código

Los comentarios encima de funciones, métodos y componentes están en su propia card: [Documentación en el código](documentacion-en-codigo.md).

## ADRs (Architecture Decision Records)

- Se escribe un ADR cuando la decisión **la necesitaría entender alguien nuevo** y **sería costoso revertirla**. Por ejemplo: la plataforma de despliegue, la base de datos estándar o la herramienta de CI.
- Formato: Contexto, Decisión, Alternativas consideradas, Consecuencias y Fecha.
- Un ADR aceptado no se edita: si la decisión cambia, se escribe uno nuevo que lo reemplaza.

## Archivos de asistentes de IA: nunca se suben

Cada persona puede usar Claude u otros asistentes en su máquina, pero **nada de lo que generan o configuran se versiona**:

| No se sube | Ejemplos |
| --- | --- |
| Instrucciones para el asistente | `CLAUDE.md`, `claude.md`, `AGENTS.md`, `.cursorrules` |
| Configuración local del asistente | `.claude/`, `.cursor/` |
| Specs, planes o notas generados por el asistente | `docs/superpowers/`, `docs/specs/` generados, `plans/` |

Se agregan al `.gitignore` de todo repo (ver [Seguridad](seguridad-y-accesos.md)). Si algo de lo que generó el asistente es útil para el equipo, **una persona lo reescribe y lo publica en la wiki o como ADR**.

Hoy incumplen: `indigo-backend` (`.claude/` y `docs/superpowers/`) y `foresight-backend` (`.claude/settings.local.json`).

## Mantener la wiki viva

- El PR que cambia cómo se instala, se corre, se configura o se integra un repo **actualiza también su página en la wiki**. Está en la [Definition of Done](definition-of-done.md).
- Las páginas de InnLab Core las mantiene el equipo de Core; las de cada proyecto, su líder técnico.
