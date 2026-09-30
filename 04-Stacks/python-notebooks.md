# Python / notebooks

📍 [InnLab](../README.md) › [Stacks](README.md) › **Python / notebooks**

> **Resumen:** los notebooks sirven para explorar, y el código reutilizable vive en módulos `.py`. Las credenciales vienen de variables de entorno o de *secrets* de Colab, nunca del notebook. Las dependencias se declaran en `requirements.txt`.

**Repos:** [foresight-scrapping](../05-Proyectos/foresight/foresight-scrapping.md)

## Estructura

```
notebooks/            → exploración y ejecución en Colab (scraping_<dominio>.ipynb)
src/<paquete>/        → funciones reutilizables (clean_text, normalize_doi, clientes de API)
tests/                → pytest para funciones puras de src/
data/                 → (no versionado) salidas y descargas
requirements.txt
.env.example
README.md
```

## Reglas

- **Nombres:** `snake_case` para archivos, módulos y funciones; `PascalCase` para clases. Nada de espacios ni sufijos `(1)` de descargas.
- **Una función que aparece en dos notebooks se mueve a `src/`.**
- **Credenciales:** `os.environ["SERPAPI_KEY"]` o `google.colab.userdata.get("SERPAPI_KEY")`. **Nunca** una key literal en una celda, porque el notebook se sube con su contenido y sus salidas.
- **Correos de contacto de APIs** (Crossref, Unpaywall): el correo del proyecto, no uno personal.
- **Antes de hacer commit, se limpian las salidas** de las celdas (`nbstripout` o *Clear all outputs*), para no subir datos ni tokens impresos.
- Los datos descargados y los resultados **no se versionan**: van a Drive o a `data/`, que está en el `.gitignore`.
- Versiones de dependencias fijadas en `requirements.txt`.

## Testing

`pytest` para las funciones puras de `src/`. Los notebooks no se testean.

## Versionado y despliegue

No se despliega ni versiona por release: la versión es el estado de `main`.

## `.gitignore` mínimo

`__pycache__/`, `.ipynb_checkpoints/`, `.venv/`, `.env`, `data/`.
