# foresight-landing

📍 [InnLab](../../README.md) › [Proyectos](../README.md) › [Foresight](README.md) › **foresight-landing**

> **Resumen:** sitio público de marketing de Foresight, en React 19 + Vite + Tailwind y JavaScript. Es contenido estático, con enlace al hub de InnLab Core para entrar.

## Ficha

| | |
| --- | --- |
| Repo | [innlab2016/foresight-landing](https://github.com/innlab2016/foresight-landing) |
| Stack | [React + Vite](../../04-Stacks/react-vite.md) · **JavaScript** · Tailwind |
| Gestor de paquetes | npm |
| Ramas | `main`, `dev` (+ `responsive`, `responsive-change-Amedida`, `feat/*` por limpiar) |
| Despliegue | En redefinición |

## Cómo correrlo

```bash
npm install
npm run dev
```

## Estructura (por tipo, se acepta en landings)

```
src/
  pages/        → una por página
  router/       → AppRouter.jsx
  components/   → una carpeta por componente
  constants/    → textos y datos estáticos
  assets/
```

## Reglas específicas de este repo

- Checklist de página nueva: crear en `pages/`, registrar la ruta en `AppRouter.jsx` y poner los datos estáticos en `constants/`.
- La carpeta y el archivo del componente coinciden en mayúsculas y minúsculas.

## Testing

No hay. Prioridad baja (contenido estático). Empezar por el router.

## Deuda técnica / plan de remediación

1. **Declarar `react-router-dom` en `package.json`**: se usa pero no está declarado, así que un `npm install` limpio falla.
2. Eliminar la duplicación `UIbuttons/` vs `UiButtons/`.
3. Borrar los archivos stub vacíos (`checkbox.jsx`, `form.jsx`, `inputs.jsx`…).
4. **Sincronizar `main` con `dev`** (~80 archivos y +3500 líneas de diferencia).
5. Commits con `styles:` → usar `style:`.
6. Limpiar ramas.
7. Documentar el despliegue cuando se defina el estándar.
8. Agregar Prettier + Husky + lint-staged.
