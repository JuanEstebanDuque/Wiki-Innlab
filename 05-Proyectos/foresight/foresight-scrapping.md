# foresight-scrapping

📍 [InnLab](../../README.md) › [Proyectos](../README.md) › [Foresight](README.md) › **foresight-scrapping**

> **Resumen:** notebooks de Python (Google Colab) que recolectan papers (Crossref, Unpaywall, OpenAlex), patentes (PatentsView) y noticias (SerpAPI) por vertical, para alimentar los informes de Foresight.

## Ficha

| | |
| --- | --- |
| Repo | [innlab2016/foresight-scrapping](https://github.com/innlab2016/foresight-scrapping) |
| Stack | [Python / notebooks](../../04-Stacks/python-notebooks.md) · Colab |
| Ramas | `main` |
| Versionado | No aplica (la versión es el estado de `main`) |
| Desplegado | No aplica (se ejecuta a mano en Colab) |

## ⚠️ Pendiente de seguridad (bloqueante)

Hay una API key de SerpAPI y correos personales en texto plano en 3 notebooks (`alfa_scraping_bmi_v3_quality`, `alfa_scraping_fintech_v2_calidad` y `alfa_scraping_agrotech_v1_calidad`), ya subidos al remoto de GitHub.
**Decisión del dueño del proyecto: por ahora no se toca; él rotará la key manualmente.**
Cuando se resuelva: rotar la key, pasarla a `userdata` de Colab o a variable de entorno, cambiar los correos por el del proyecto y evaluar si se purga el historial (ver [Seguridad](../../01-Estandares/seguridad-y-accesos.md)).

## Contenido actual

| Notebook | Qué hace |
| --- | --- |
| `alfa_scraping_base_v6` | Base común de scraping |
| `alfa_scraping_<vertical>_*` (agrotech, biotech, bmi, fintech) | Scraping por vertical |
| `completar_abstracts_keywords` | Enriquece abstracts y keywords |
| `descargar_noticias_pdf_FINAL_fixed` | Descarga noticias en PDF |
| `search_for papers in crossref`, `Scraping` | Exploración |

## Reglas específicas de este repo

- Estructura objetivo `notebooks/` + `src/` + `tests/`, según [Python / notebooks](../../04-Stacks/python-notebooks.md).
- La clase `Config` de cada notebook solo tiene **nombres** de variables, nunca valores.
- Clases en PascalCase (`CrossrefSearcher`, `NewsScraper`).

## Accesos necesarios

SerpAPI (cuota limitada, tiene dueño designado), Google Colab/Drive, Crossref/Unpaywall (con el correo del proyecto) y PatentsView (API key).

## Deuda técnica / plan de remediación

1. **Seguridad**: ver arriba.
2. Crear README, `.gitignore` y `requirements.txt`.
3. Renombrar los notebooks a `scraping_<vertical>.ipynb`, sin sufijos `(1) (1)` ni espacios.
4. Extraer las funciones repetidas (`clean_text`, `normalize_doi`…) a `src/`.
5. Borrar la carpeta `Descarga-Papers-VS-2026…` que generó Google Drive, o moverla fuera del repo.
6. Limpiar las salidas de las celdas antes de hacer commit (`nbstripout`).
7. Adoptar Conventional Commits (hoy hay 2 commits genéricos).
8. Documentar cómo llegan los resultados a la BD de Foresight.
