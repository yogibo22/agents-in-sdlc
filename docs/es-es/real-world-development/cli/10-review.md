---
title: "Lección 10 - Cierre y siguientes pasos"
description: "Resume el flujo de trabajo de Copilot CLI, las dos pull requests, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI a lo largo de un flujo continuo de Tailspin Toys. Has:

- preparado un Codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia inicial de filtrado.
- añadido valoraciones por estrellas, revisado el resultado en un navegador reenviado y combinado manualmente tu primera pull request (PR).
- empezado desde la incidencia de filtrado, definido el enfoque en el modo Plan, creado la funcionalidad en el modo Autopilot y revisado el resultado en el modo Interactive.
- guiado al agente con instrucciones personalizadas, personalizado la habilidad existente `quality-checks` y usado la habilidad para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- añadido el servidor Model Context Protocol (MCP) de Playwright y usado el servidor para explorar el filtrado en un navegador real.
- creado y seleccionado un agente personalizado de aseguramiento de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas en el navegador.
- revisado el cambio completo de filtrado y autorizado Agent Merge para la PR de filtrado.
- explorado comandos de barra para el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

## Qué has entregado

El taller tiene dos hitos de PR, cada uno en su propia rama desde `main` actualizado:

1. **Valoraciones por estrellas:** muestra la `starRating` existente y un estado explícito de falta de valoración en las fichas de juego.
2. **Filtrado y flujo de calidad:** implementa el filtrado, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas asociadas.

Desde la planificación del filtrado hasta la apertura de su PR, usaste la misma conversación y la misma rama. Combinamos ese trabajo en una sola PR para simplificar el taller.

## Diferentes tipos de verificación

Has comprobado el código de varias formas: con pruebas automatizadas, mediante tu propia inspección en el navegador y con la exploración del navegador de Copilot a través de MCP. La habilidad `quality-checks` ejecutó pruebas unitarias, lint y comprobaciones de tipos, e informó de los resultados con el nuevo formato. Antes de la PR, QA reunió esos resultados con una revisión de los requisitos y la cobertura de pruebas.

Las pruebas añadidas deberían cubrir carencias reales; una ejecución de QA que no requiera pruebas nuevas puede ser correcta. Revisa el código y las pruebas antes de autorizar la combinación, y actualiza las pruebas afectadas después de hacer cambios.

## Buenas prácticas

El contexto y las herramientas que proporcionas a Copilot dan forma a su trabajo. En este taller, actualizaste instrucciones, personalizaste una habilidad, creaste un perfil de QA y configuraste un servidor MCP. Reutiliza estas personalizaciones en distintas conversaciones y ajústalas a medida que cambien las necesidades del equipo. Las instrucciones establecen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios reales y los resultados de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Usa **Plan** para pensar en un enfoque antes de construir, **Interactive** para mantener el control de los cambios concretos y **Autopilot** para las tareas bien delimitadas. Elige un modelo más rápido para las ediciones rutinarias y uno más capaz para los trabajos complejos.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* cambia notablemente el resultado.

## Más cosas que explorar

Ya has completado el flujo de trabajo principal. Estas son algunas funciones más de CLI que vale la pena explorar:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para hablar de un problema y obtener otra perspectiva.
- `/fleet` para orquestar subtareas independientes en paralelo.
- `/worktree` para aislar otra tarea.
- `/delegate` para enviar una tarea a Copilot cloud agent.

## Pasos siguientes

La mejor forma de mejorar con cualquier herramienta es seguir usándola. Úsala para código de producción, para proyectos personales o para esa pequeña aplicación que llevas años queriendo crear pero nunca has llegado a empezar. Comparte lo que aprendas con tu equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación de GitHub Copilot][app] o el [taller del agente en la nube de Copilot][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar pull requests con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
