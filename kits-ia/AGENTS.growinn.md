# AGENTS.md — Growinn Index / Inndex (InnLab)

Eres un asistente de código trabajando en **Growinn Index (Inndex)**: encuestas de diagnóstico para empresas por áreas y jerarquías (operativo, táctico, estratégico). **Este producto será reemplazado por Índigo**: está en mantenimiento.

## Regla principal
- Solo se corrige lo que bloquee la operación. **No propongas refactors, migraciones de stack, reestructuración de carpetas ni dependencias nuevas.**

## Repo
- `-Growinn-index-test` (monorepo, npm):
  - `Backend/`: Express 4 + Supabase + MailerSend + ExcelJS/xlsx. Rutas en `routes/`, lógica de puntajes en `constants/` (`calculateScore.js`, `categorizeAnswer.js`, `questionMap`), correo en `mail/`.
  - `Frontend/`: React 19 + Vite + Tailwind 4, JavaScript.
- Auth con Supabase Auth (no está integrado con InnLab Core y no se va a integrar).

## Si tocas algo
- Si cambias lógica de cálculo de puntajes, agrega tests de esa lógica.
- La lógica de `routes/` llama funciones; no metas lógica nueva dentro del handler.
- Configuración desde `.env`; CORS desde variable de entorno.
- Commits Conventional Commits en inglés; rama `fix/<issue>-<descripcion>`; PR con `Fixes #n`.

## Nunca
- Commitear `.env`, keys de Supabase o MailerSend, ni datos de empresas.
- Commitear `AGENTS.md`, `CLAUDE.md` ni `.claude/`.
