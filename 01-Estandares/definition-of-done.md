# Definition of Done

📍 [InnLab](../README.md) › [Estándares](README.md) › **Definition of Done**

> **Resumen:** una tarea está terminada cuando está mergeada, probada, documentada y lista para desplegar. Que funcione en tu máquina no basta.

- [ ] El código cumple los [Estándares](README.md) y las reglas de su [stack](../04-Stacks/README.md).
- [ ] Hay tests para la lógica nueva o modificada, y pasan.
- [ ] Las funciones y componentes públicos nuevos o modificados tienen su comentario de [documentación](documentacion-en-codigo.md).
- [ ] Lint (sin advertencias), build y pruebas pasan en el CI, con la cobertura mínima de [CI y calidad](ci-y-calidad.md).
- [ ] La versión está subida y el `CHANGELOG.md` tiene su entrada.
- [ ] No hay secretos, datos personales ni URLs de ambiente hardcodeadas.
- [ ] El PR fue revisado, **validado en staging** (cuando exista el ambiente) y aprobado por al menos 1 persona distinta al autor.
- [ ] El issue quedó cerrado por el PR (`Fixes #n`).
- [ ] Si cambió cómo se instala, se corre, se configura o se despliega: README, `.env.example` y página de la wiki actualizados.
- [ ] Si hubo una decisión de arquitectura: ADR escrito.
- [ ] Promovido a producción (o, mientras no haya plataforma, con el simulacro de promoción en verde). En backends, Quality Gate de SonarQube Cloud en verde sobre `main`.
