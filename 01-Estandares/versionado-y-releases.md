# Versionado y releases

📍 [InnLab](../README.md) › [Estándares](README.md) › **Versionado y releases**

> **Resumen:** cada repo tiene su propia versión con formato [SemVer](https://semver.org/lang/es/) `MAJOR.MINOR.PATCH`. **Cada PR que se fusiona es un release:** trae su versión subida y su entrada en `CHANGELOG.md`, y al fusionarlo la promoción automática lleva `dev` a `main`, despliega a producción y etiqueta `vX.Y.Z`. El número sale del tipo de cambio (tabla de abajo).

## Qué significa cada número

```
  1  .  4  .  2
  │     │     └── PATCH: correcciones que no cambian cómo se usa
  │     └──────── MINOR: funcionalidad nueva que no rompe lo existente
  └────────────── MAJOR: cambio que rompe compatibilidad
```

| Sube | Cuándo | Qué se reinicia | Ejemplo |
| --- | --- | --- | --- |
| **PATCH** | Solo `fix:` desde el último release | Nada | `1.4.1` → `1.4.2` |
| **MINOR** | Al menos un `feat:` y nada que rompa | PATCH vuelve a 0 | `1.4.2` → `1.5.0` |
| **MAJOR** | Al menos un cambio que rompe (`feat!:` o `BREAKING CHANGE:`) | MINOR y PATCH vuelven a 0 | `1.5.0` → `2.0.0` |

**Qué cuenta como "romper":**
- **Backend / API:** se quita o se renombra un endpoint o un campo, cambia el formato de una respuesta, un campo pasa a ser obligatorio. Cualquier cosa que obligue a los consumidores (un frontend u otro backend) a cambiar su código.
- **Frontend:** cambia algo de lo que dependen otros, como las rutas públicas a las que redirige el hub de Core o las variables de entorno que necesita para arrancar. Un rediseño visual **no** es MAJOR por sí solo.

## 0.0.1, 0.1.0 o 1.0.0: dónde estamos

| Versión | Qué significa | Cuándo usarla |
| --- | --- | --- |
| `0.0.x` | Esqueleto: el proyecto recién creado, sin nada usable | Solo lo que genera la plantilla (`0.0.0` o `0.0.1`). **No se publica un release así** |
| `0.1.0` | **Primera versión usable**, aunque incompleta | El primer release que se despliega (a staging o producción) |
| `0.x.y` | En desarrollo activo: la API **puede cambiar sin aviso** | Mientras el producto se está construyendo. Los cambios que rompen suben **MINOR** (`0.3.0` → `0.4.0`) y lo demás sube PATCH |
| `1.0.0` | **Estable**: se compromete a no romper sin subir MAJOR | Cuando se cumplen los criterios de abajo |

**Criterios para pasar a `1.0.0`:**
- [ ] Está en producción con usuarios reales.
- [ ] Hay otros que dependen de su contrato (por ejemplo, la API de Core la consumen los productos).
- [ ] Los flujos principales están completos y con tests.
- [ ] El equipo se compromete a avisar y subir MAJOR antes de romper algo.

**Estado de los repos hoy:** todos están en `0.0.0` o `0.0.1` (lo que dejó la plantilla) y ninguno tiene tags. **(Propuesta)** El primer release bajo este estándar es `0.1.0`. Core y Foresight, que ya tienen usuarios, evalúan pasar a `1.0.0` cuando se cumplan los criterios.

## Cómo se hace un release

Con el flujo de [Despliegue y CI/CD](despliegue-y-ambientes.md), cada cambio validado en staging sale a producción al fusionarlo. Por eso **el release va dentro del propio PR**:

1. El autor decide el número según la tabla de arriba y, en el mismo PR:
   - actualiza `"version"` en `package.json` (o su equivalente),
   - agrega la entrada en `CHANGELOG.md`.
2. Quien revisa valida el cambio en staging y lo fusiona a `dev` (squash).
3. **Automático:** la promoción une `dev` en `main` con **merge commit**, despliega a producción, crea el tag `vX.Y.Z` con la versión del `package.json` y publica el Release en GitHub con las notas del CHANGELOG.
4. Si producción falla, el rollback vuelve a desplegar el tag anterior (solo en producción).

**Hoy**, sin plataforma de despliegue, el paso 3 es el **simulacro de promoción**: no publica ni etiqueta. Hasta que exista la promoción real, la unión `dev` → `main` y el tag se hacen a mano:
```bash
git checkout main && git pull
git merge --no-ff origin/dev
git push origin main
git tag v0.4.0
git push origin v0.4.0
```

### Hotfix

Un bug crítico en producción no espera al siguiente release:
1. Rama `hotfix/<issue>-<desc>` desde `main`.
2. PR a `main`: sube **PATCH** (`v1.4.2` → `v1.4.3`), con tag y CHANGELOG.
3. Se lleva el mismo cambio a `dev` (PR o merge de `main` a `dev`) para no perderlo.

## CHANGELOG.md

Se escribe para humanos y se agrupa por tipo. Se arma a partir de los títulos de los PRs (que ya siguen [Conventional Commits](commits.md)):

```markdown
## [0.4.0] - 2026-10-15
### Nuevo
- Exportar informe como PDF (#42)
### Corregido
- Lista vacía de verticales ya no rompe el dashboard (#57)
### ⚠️ Cambios que rompen
- `GET /api/reports` ahora pagina los resultados (#60)
```

**(Propuesta)** Más adelante, herramientas como *release-please* o *semantic-release* pueden calcular la versión y escribir el CHANGELOG solas a partir de los commits. Por eso importa que los commits sigan el formato desde ya.

## Reglas

- **Cada repo versiona por separado.** No hay "versión de Foresight": hay versión de `foresight-backend`, de `foresight-user-space`, etc.
- **La versión de la API de Core es la más importante del ecosistema:** un MAJOR ahí afecta a todos los productos y se avisa antes (ver [API interna](../03-InnLab-Core/api-interna.md)).
- Los tags siempre llevan `v` (`v1.2.0`) y **nunca se mueven ni se borran** una vez publicados.
- Los repos que no se despliegan (como `foresight-scrapping`) no versionan. Lo dicen en su página.
