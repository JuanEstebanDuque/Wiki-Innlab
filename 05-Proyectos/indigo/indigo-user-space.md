# indigo-user-space

📍 [InnLab](../../README.md) › [Proyectos](../README.md) › [Índigo](README.md) › **indigo-user-space**

> **Resumen:** frontend de Índigo en **Vue 3 + Vite + TypeScript**. Vue se eligió por velocidad de desarrollo, rendimiento y una curva de aprendizaje fácil para todo el equipo, y **así se queda**.

## Ficha

| | |
| --- | --- |
| Repo | [innlab2016/indigo-user-space](https://github.com/innlab2016/indigo-user-space) |
| Stack | [Vue + Vite](../../04-Stacks/vue-vite.md) · Vue 3 · TypeScript (`vue-tsc`) |
| Gestor de paquetes | npm |
| Estado | Base de Vite creada en local; **sin commits todavía** |

## Cómo correrlo

```bash
npm install
cp .env.example .env
npm run dev
```

## Pasos para dejarlo en el estándar

1. **Primer commit en `main` y crear `dev`**, y proteger las dos ramas (ver [Git y ramas](../../01-Estandares/git-y-ramas.md)).
2. `.gitignore` con `.env` y los archivos de asistentes de IA (`.claude/`, `CLAUDE.md`…).
3. Agregar Vue Router, ESLint (`eslint-plugin-vue`) + Prettier y un framework de tests (ver [Testing](../../02-Testing/README.md)). Crear los scripts `lint`, `format` y `test`, que hoy no existen.
4. Estructura por feature según [Vue + Vite](../../04-Stacks/vue-vite.md). Borrar el `HelloWorld.vue` de la plantilla.
5. **Implementar la recepción del SSO de InnLab Core** antes de construir pantallas protegidas (ver [Autenticación y SSO](../../03-InnLab-Core/autenticacion-y-sso.md)). No se construye login propio.
6. Reemplazar el README de la plantilla de Vite por un README corto que enlace a esta página.
7. Crear `.env.example` con las variables `VITE_*` (URL del backend de Índigo, URL del hub y de la API de Core).
