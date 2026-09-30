# Commits

📍 [InnLab](../README.md) › [Estándares](README.md) › **Commits**

> **Resumen:** [Conventional Commits](https://www.conventionalcommits.org/es/v1.0.0/). El formato es `<tipo>(alcance opcional): resumen`, en inglés, en imperativo y de máximo 72 caracteres.

## Formato

```
<tipo>(<alcance>): <resumen en imperativo>

<cuerpo opcional: qué y por qué, líneas de ≤72 caracteres>

<footer opcional: Fixes #42 / Refs #40 / BREAKING CHANGE: ...>
```

## Tipos permitidos

| Tipo | Uso |
| --- | --- |
| `feat` | Funcionalidad nueva para el usuario |
| `fix` | Corrección de un bug |
| `refactor` | Cambio interno que no altera el comportamiento |
| `perf` | Mejora de rendimiento |
| `test` | Agregar o corregir tests |
| `docs` | Solo documentación |
| `style` | Formato o estilos visuales, sin lógica (**no** `styles:`) |
| `chore` | Dependencias, configuración, tooling |
| `ci` | Pipelines de CI/CD |
| `build` | Sistema de build y empaquetado |

- El **alcance** es el módulo o área afectada: `feat(auth): ...`, `fix(reports): ...`.
- Un cambio que rompe compatibilidad lleva `!` después del tipo (`feat(api)!: ...`) y además `BREAKING CHANGE:` en el footer.
- Para cerrar el issue al hacer merge se usa `Fixes #n`, `Closes #n` o `Resolves #n` en el footer. Para solo referenciarlo, `Refs #n`.

## Ejemplos

✅ Bien
```
feat(auth): add cognito token refresh
fix(reports): handle empty vertical list
docs: document required env vars in readme
refactor(diagnosticador): extract retry logic from claude client
```

❌ Mal (todos son ejemplos reales de los repos)

| Mensaje | Problema |
| --- | --- |
| `Correos`, `tabla`, `try`, `arreglo`, `server` | No dicen qué cambió ni tienen tipo |
| `Add files via upload` | Es el mensaje por defecto de la web de GitHub |
| `Refactor OpenAlex and Tavily API calls...; enhance...; update...; modify...` | Son 4 cambios en un solo commit: deberían ser commits separados |
| `styles: ...` | El tipo correcto es `style` |

## Reglas

- **Un commit = un cambio lógico.** Si el resumen necesita "y" o ";", probablemente son dos commits.
- Nada de commits "WIP" en ramas que se van a revisar. Si los hubo, desaparecen con el squash merge.
- **(Propuesta)** Se valida automáticamente con `commitlint` en un hook `commit-msg` (ver [Dependencias y herramientas](dependencias-y-herramientas.md)).
