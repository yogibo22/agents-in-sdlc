---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa juntos el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, la actualización de las instrucciones, la actualización de la habilidad, el perfil de aseguramiento de calidad (QA) y las pruebas están en una sola rama. Es el momento de revisarlo todo junto y abrir una pull request. Combinaste manualmente la pull request de valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, la actualización de las instrucciones, la actualización de la habilidad y el agente de QA en varias PR. Para simplificar el taller, has mantenido todo el flujo de trabajo de filtrado y calidad en una sola conversación y rama, y todo ese trabajo se incluirá en esta PR.

En esta lección, aprenderás a:

- entender qué es Agent Merge y cómo automatiza el ciclo de vida de la combinación.
- inspeccionar el cambio completo de la funcionalidad y las pruebas de verificación.
- crear la PR de filtrado.
- activar Agent Merge solo después de la revisión y confirmar que la PR se ha combinado.

## Escenario

A lo largo del flujo de trabajo de filtrado, has usado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR y mantener la autorización de combinación bajo el control del desarrollador.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para integrar una pull request. Cuando lo activas, Copilot trabaja en los elementos que bloquean la PR: corrige las comprobaciones fallidas de integración continua (CI), responde a comentarios de revisión y vuelve a aplicar los cambios cuando es necesario. Después, activa la combinación automática de GitHub si el repositorio lo permite.

Hasta ahora has seleccionado **Merge pull request** por tu cuenta. Agent Merge puede encargarse de esta parte. Revisa el trabajo y decide si está listo antes de activarlo.

## Usa Agent Merge para gestionar la PR

Ya has creado todo el código. Ahora revisémoslo, creemos la PR y dejemos que Agent Merge se encargue del resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo del agente con `/agent`.
3. Selecciona **Default** en la lista de opciones y pulsa <kbd>Enter</kbd>.
4. Crea una nueva PR con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todo esté correcto, combinará la PR.
7. Confirma que aparece un mensaje de Copilot similar a "PR #14 was squash-merged successfully."

> [!IMPORTANT]
> Agent Merge no evita las aprobaciones obligatorias, la protección de ramas, las colas de combinación, las restricciones de configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo y completa manualmente la combinación revisada cuando el repositorio lo permita.

## Resumen y pasos siguientes

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la validación del código, y ahora el proceso de pull request. En esta lección:

- aprendiste qué es Agent Merge y cómo automatiza el ciclo de vida de la combinación.
- inspeccionaste el cambio completo de la funcionalidad y las pruebas de verificación.
- creaste la PR de filtrado.
- activaste Agent Merge solo después de la revisión y confirmaste que la PR se había combinado.

A continuación, [explorarás más comandos de barra útiles de Copilot CLI][next-lesson] para el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

## Recursos

- [Gestionar pull requests con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
