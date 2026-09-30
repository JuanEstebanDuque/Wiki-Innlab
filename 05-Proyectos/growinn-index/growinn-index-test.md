# -Growinn-index-test

📍 [InnLab](../../README.md) › [Proyectos](../README.md) › [Growinn Index](README.md) › **-Growinn-index-test**

> **Resumen:** monorepo de Inndex, con `Backend/` (Express + Supabase + MailerSend) y `Frontend/` (React 19 + Vite + Tailwind 4, en JS). **Se mantiene tal cual hasta que Índigo lo reemplace.**

## Ficha

| | |
| --- | --- |
| Repo | [innlab2016/-Growinn-index-test](https://github.com/innlab2016/-Growinn-index-test) |
| Stack | Backend: [Express](../../04-Stacks/express-node.md) 4 · Supabase · MailerSend · ExcelJS / xlsx. Frontend: [React + Vite](../../04-Stacks/react-vite.md) (JS) · Tailwind 4 |
| Gestor de paquetes | npm |
| Ramas | `main`, `cdg` |

## Cómo correrlo

```bash
cd Backend && npm install && cp .env.example .env && npm run dev
cd Frontend && npm install && npm run dev
```

## Estructura

```
Backend/
  server.js         → Express, CORS, Helmet, HTTP + HTTPS local
  routes/           → survey, excelroute, uploadExcel, analizarResultados
  constants/        → calculateScore, categorizeAnswer, questionMap (lógica de puntajes)
  mail/             → mailSender + template.html
  supabase/         → cliente
Frontend/src/
  components/       → ~40 carpetas de componentes
```

## Variables de entorno

- **Backend:** `PORT`, `NODE_ENV`, `SUPABASE_URL`, `SUPABASE_ANON_KEY`, `SUPABASE_SERVICE_ROLE_KEY` (el código también lee `SUPABASE_SERVICE_ROLE`), `MAILERSEND_API_KEY`, `FROM_EMAIL`, `BASE_URL`, `FRONTEND_URL`, `ALLOWED_ORIGINS`, `ADDITIONAL_ORIGINS`, `FRONTEND_ORIGIN`.
- **Frontend:** `VITE_API_URL`, `VITE_SUPABASE_URL`, `VITE_SUPABASE_ANON_KEY`.

## Hallazgos (referencia para la migración a Índigo)

No se corrigen salvo que bloqueen la operación. Quedan documentados para que Índigo no los repita:

- La lógica de puntajes (`constants/calculateScore.js`, `categorizeAnswer.js`) no tiene tests. Es la parte a reimplementar con cuidado en Índigo.
- `.env.example` desactualizado respecto al código, y nombres de variables duplicados (CORS, Supabase).
- Dependencias que imitan módulos nativos (`fs`, `path`, `https`) y otras posiblemente sin uso (`sequelize`, `pg`).
- Nombres de componentes inconsistentes (`Card-login.jsx`, `form_areas.jsx`, `Navbar.jsx`).
- Commits sin formato y muchos commits de merge.
- Autenticación propia con Supabase: en Índigo pasa por InnLab Core.
