# Documentación en el código

📍 [InnLab](../README.md) › [Estándares](README.md) › **Documentación en el código**

> **Resumen:** toda función, método, clase o componente **público** lleva encima un comentario corto que dice **qué hace y para qué sirve**. Se usa el formato estándar de cada lenguaje (JSDoc/TSDoc en TypeScript y JavaScript, docstrings en Python), así el editor lo muestra al pasar el mouse. Se escribe en español. En el código existente se agrega al tocarlo.

## Qué se documenta

| Se documenta siempre | No hace falta |
| --- | --- |
| Funciones y métodos exportados o públicos | Funciones privadas triviales de 1-3 líneas cuyo nombre ya lo dice todo |
| Métodos de **services**, **controllers**, **guards**, **clientes de API** | Getters y setters simples |
| **Composables** (Vue) y **hooks** (React) | Código generado |
| **Componentes** (una línea sobre para qué sirven, y sus props) | Tests (el nombre del test ya describe el comportamiento) |
| Clases, y funciones de Python en `src/` | Cada línea del código |

Una función privada que no es obvia (un cálculo o una regla de negocio) también se documenta.

## Qué lleva el comentario

1. **Qué hace y para qué sirve**, en 1-2 líneas. Obligatorio.
2. **Parámetros y retorno**, solo si los tipos no bastan (unidades, rangos, formato, qué significa `null`).
3. **Errores que lanza**, si lanza alguno a propósito.
4. Contexto de negocio, si hace falta: por qué existe o de dónde sale una regla.

**No lleva:** cómo lo hace paso a paso (eso lo dice el código), ni el historial de cambios (eso lo dice git).

## Ejemplos

**Backend (TypeScript / NestJS)**
```ts
/**
 * Calcula el puntaje de un área a partir de las respuestas Likert-6 de sus colaboradores.
 * Se usa en el dashboard de resultados por área.
 *
 * @param answers - Respuestas de todos los colaboradores del área (valores 1 a 6).
 * @returns Puntaje normalizado entre 0 y 100.
 * @throws BadRequestException si la lista está vacía.
 */
calculateAreaScore(answers: number[]): number { … }
```

**Frontend (React: hook)**
```ts
/**
 * Maneja la sesión del usuario obtenida por el SSO de InnLab Core.
 * Expone el usuario actual y un logout que cierra la sesión en todo el ecosistema.
 */
export function useAuth() { … }
```

**Frontend (Vue: composable y componente)**
```ts
/**
 * Carga un diagnóstico y calcula el progreso de las respuestas.
 * Lo usan las vistas del wizard y el resumen del diagnóstico.
 *
 * @param diagnosticId - ID del diagnóstico en el backend de Índigo.
 */
export function useDiagnostic(diagnosticId: string) { … }
```
```vue
<script setup lang="ts">
/**
 * Tarjeta con el resumen de un diagnóstico (nombre, estado y avance).
 * Se muestra en el listado de diagnósticos del profesor.
 */
defineProps<{
  /** Diagnóstico a mostrar. */
  diagnostic: DiagnosticSummary
  /** Si es true, muestra las acciones de edición. */
  editable?: boolean
}>()
</script>
```

**Python**
```python
def normalize_doi(raw: str) -> str | None:
    """Normaliza un DOI a su forma canónica (minúsculas, sin prefijo https://doi.org/).

    Se usa para deduplicar papers que vienen de Crossref y OpenAlex.
    Devuelve None si el texto no contiene un DOI válido.
    """
```

## Reglas

- **Un comentario desactualizado es un bug.** Si cambias lo que hace una función, actualizas su comentario en el mismo PR. El revisor lo verifica.
- **En código existente no se exige retroactivamente de golpe:** cuando tocas una función pública que no tiene comentario, se lo agregas en ese PR (regla del boy scout).
- **(Propuesta)** Validarlo con lint: `eslint-plugin-jsdoc` en TS/JS y `pydocstyle` en Python, en modo `warn` al principio.
