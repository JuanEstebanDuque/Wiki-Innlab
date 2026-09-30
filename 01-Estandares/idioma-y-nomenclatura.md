# Idioma y nomenclatura

📍 [InnLab](../README.md) › [Estándares](README.md) › **Idioma y nomenclatura**

> **Resumen:** el código (identificadores, archivos, commits y ramas) va en inglés, y la documentación y los textos de negocio en español. Cada tipo de elemento tiene una sola convención de nombre.

## Idioma

| Qué | Idioma |
| --- | --- |
| Variables, funciones, clases, archivos, carpetas | Inglés |
| Commits, ramas, títulos de PR | Inglés |
| README, ADRs, esta wiki, descripción de PRs | Español |
| Comentarios en el código | Español (explican el *por qué* de negocio) |
| Textos que ve el usuario final (UI, correos) | Español, o lo que defina el producto |

**No se mezclan idiomas en un mismo identificador:** `pagosAmedida.jsx` ❌ → `CustomPayments.jsx` ✅.

**(Propuesta) Términos de dominio.** Algunos conceptos del negocio no tienen buena traducción (por ejemplo, `diagnosticador` o `macro_categorias` en Índigo). Hay dos opciones: traducirlos (`diagnostic`, `macro_categories`) o permitirlos en español, siempre que estén listados en el [Glosario](../06-Referencia/glosario.md). Ver [Decisiones pendientes](../06-Referencia/decisiones-pendientes.md).

## Convenciones de nombres

| Elemento | Convención | Ejemplo |
| --- | --- | --- |
| Carpetas | kebab-case | `custom-made/`, `company-membership/` |
| Archivos TS/JS (backend, utilidades) | kebab-case con sufijo de rol | `reports.service.ts`, `create-company.dto.ts` |
| Componentes React | PascalCase (el archivo se llama igual que el componente) | `ProductCard.tsx` |
| Hooks React | camelCase con prefijo `use` | `useAuth.ts` |
| Clases | PascalCase con sufijo de rol | `ReportsService`, `CognitoGuard` |
| Variables y funciones | camelCase | `getCompanyById` |
| Constantes globales | UPPER_SNAKE_CASE | `PRODUCT_CONFIG` |
| Variables de entorno | UPPER_SNAKE_CASE, prefijadas por servicio cuando haya ambigüedad | `FORESIGHT_DB_HOST`, `VITE_API_URL` |
| Tablas y columnas de BD | snake_case, tablas en plural | `company_memberships.created_at` |
| Rutas REST | plural, kebab-case, con prefijo `/api` (sin versión) | `/api/custom-made-assets` |
| Ramas | `<tipo>/<issue>-kebab-case` | `feat/42-pdf-export` |
| Python: módulos y funciones | snake_case | `normalize_doi()` |
| Python: clases | PascalCase | `CrossrefSearcher` |

**Nombres que no se usan** (hay ejemplos en los repos actuales):
- Carpetas duplicadas que solo cambian en mayúsculas o minúsculas (`UIbuttons/` vs `UiButtons/`). En Windows y macOS se pisan entre sí.
- Varios estilos de nombre en la misma carpeta (`Card-login.jsx`, `form_areas.jsx` y `Navbar.jsx` juntos).
- Sufijos de descarga (`archivo (1) (1).ipynb`) y nombres con espacios.
