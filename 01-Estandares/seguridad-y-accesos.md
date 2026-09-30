# Seguridad, secretos y accesos

📍 [InnLab](../README.md) › [Estándares](README.md) › **Seguridad, secretos y accesos**

> **Resumen:** ningún secreto entra a git. Cada servicio externo tiene un dueño que da y quita accesos, y si un secreto se filtra, primero se rota y después se limpia.

## Secretos y variables de entorno

- Los secretos van en un archivo que se llama **solo `.env`** (nunca versionado). No se usan variantes como `.env.local`, `.env.development` o `.env.production`: cada ambiente tiene su propio `.env`, o sus variables en la plataforma de despliegue. En el repo se versiona `.env.example` con **los nombres** de las variables y valores de ejemplo, y se mantiene al día.
- `.gitignore` mínimo de todo repo:

```gitignore
# Secretos
.env

# Asistentes de IA (nunca se versionan)
.claude/
CLAUDE.md
claude.md
docs/superpowers/
```
- Tampoco se versionan los archivos de asistentes de IA (`.claude/`, `CLAUDE.md`…; ver [Documentación](documentacion.md)), la configuración personal del editor ni los certificados (`*.pem`, `*.key`).
- En los ambientes desplegados, las variables se configuran en la plataforma de despliegue o en su gestor de secretos. Nunca van en archivos versionados ni en el código.
- **No se hardcodean URLs de ambiente** (orígenes CORS, URLs de APIs): se leen de variables de entorno.

## Si un secreto se filtra

1. **Rotarlo de inmediato** en el proveedor. Una key que llegó a un repo público se da por comprometida, aunque se borre después.
2. Moverlo a una variable de entorno y desplegar.
3. Evaluar si se purga del historial (`git filter-repo`). Esto reescribe la historia, así que se coordina con el equipo.
4. Registrarlo en la página del repo afectado, en "Deuda técnica", hasta que se cierre.

## Cuentas de acceso

- Cada persona usa **su propia cuenta**. No hay usuario compartido por el equipo.
- Se da el **mínimo privilegio**: quien solo desarrolla no tiene credenciales de producción.
- Cuando alguien sale del equipo, se le revocan todos los accesos el mismo día.

## Dueños de cuentas

> _Los dueños se asignan internamente. Esta tabla se completa cuando se definan._

| Servicio | Lo usan | Dueño |
| --- | --- | --- |
| GitHub org `innlab2016` | Todos | _por definir_ |
| Plataforma de despliegue (en definición) | Todos | _por definir_ |
| AWS Cognito (SSO) | Core, Foresight | _por definir_ |
| AWS S3 | Foresight | _por definir_ |
| Bases de datos (servidores de la universidad, en definición) | Todos | _por definir_ |
| Supabase | Growinn, Índigo | _por definir_ |
| MailerSend | Growinn | _por definir_ |
| OpenRouter / Anthropic | Índigo | _por definir_ |
| Tavily, OpenAlex | Índigo | _por definir_ |
| SerpAPI, Crossref, Unpaywall, PatentsView | Foresight (scrapping) | _por definir_ |

## Comunicación entre servicios

- Cada backend de producto se identifica ante InnLab Core con **su propia key interna** (`INTERNAL_KEY_<PRODUCTO>`). Las keys nunca se comparten ni van en un frontend.
- Detalle en [InnLab Core → API interna](../03-InnLab-Core/api-interna.md).
