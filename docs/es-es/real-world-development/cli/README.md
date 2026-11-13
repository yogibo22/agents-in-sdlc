---
slug: es-es/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Construye, valida y entrega dos cambios de Tailspin Toys mientras aprendes los modos de Copilot CLI, personalizaciones, herramientas MCP y automatización de pull requests."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** lleva GitHub Copilot a tu terminal como asistente de programación con agentes. Explora bases de código, genera código, ejecuta comandos y se conecta a herramientas externas desde la línea de comandos, para que puedas mantener el flujo sin cambiar a un editor gráfico.

El taller sigue un flujo continuo de trabajo de Tailspin Toys:

1. Prepara el proyecto en GitHub Codespaces, instala Copilot CLI y prepárate.
2. Haz un cambio enfocado de valoración por estrellas, revísalo en el navegador y fusiona tu primera pull request (PR) manualmente.
3. Empieza con la incidencia de filtrado, define el enfoque en el modo Plan, construye en modo Autopilot y revísalo en modo Interactive.
4. Actualiza las instrucciones del repositorio y aplícalas al trabajo de filtrado.
5. Personaliza la habilidad existente `quality-checks` y úsala para ejecutar las comprobaciones del proyecto.
6. Añade el servidor Model Context Protocol (MCP) de Playwright y úsalo para explorar el filtrado en un navegador.
7. Crea un agente personalizado de control de calidad (QA) y úsalo para revisar requisitos, cobertura y pruebas de verificación.
8. Revisa el cambio completo de filtrado y usa Agent Merge para la PR de filtrado.
9. Explora comandos de barra útiles para el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

Para mantener el taller centrado, crearás dos PR: una para las valoraciones por estrellas y otra para el filtrado con las actualizaciones de instrucciones, la mejora de la habilidad, el perfil de QA y las pruebas. El flujo de filtrado y calidad comparte una conversación y una rama para que puedas seguir construyendo sobre tu trabajo a medida que exploras cada herramienta.

## Lecciones

| Lección | Tema | Descripción |
| ------- | ---- | ----------- |
| [0. Requisitos previos][ex0] | Configuración | Crea tu repositorio y tu Codespace |
| [1. Instalar Copilot CLI][ex1] | Instalación | Instala y autentica Copilot CLI y prepárate |
| [2. Añadir valoraciones por estrellas: una mejora rápida][ex2] | Primer cambio | Muestra las valoraciones existentes y el valor alternativo para nulo, y combina tu primera PR |
| [3. Modos de agente: Plan y Autopilot][ex3] | Modos de agente | Planifica la funcionalidad desde la incidencia, créala con Autopilot y revísala en modo Interactive |
| [4. Guiar a Copilot con instrucciones personalizadas][ex4] | Contexto | Explora y actualiza las instrucciones y aplícalas al filtrado |
| [5. Personalizar y usar una habilidad `quality-checks`][ex5] | Comprobaciones reutilizables | Explora la habilidad existente, cambia el formato del informe y ejecútala |
| [6. Validar la funcionalidad con Playwright MCP][ex6] | Observación del navegador | Configura MCP en la CLI e inspecciona el comportamiento del filtrado |
| [7. Crear y usar un agente QA][ex7] | Requisitos y cobertura | Crea y selecciona un perfil especializado y recopila las pruebas finales de verificación |
| [8. Crear y combinar la PR de la funcionalidad][ex8] | Revisión y combinación | Revisa el cambio completo, crea la PR y usa Agent Merge |
| [9. Comandos de barra en GitHub Copilot CLI][ex9] | Funciones de la CLI | Explora el contexto, los modelos, el uso compartido y la delegación opcional a la nube |
| [10. Repaso y pasos siguientes][ex10] | Repaso | Repasa el flujo de trabajo, las personalizaciones reutilizables y otros recursos |
| [Opcional: Incorporar Foundry][foundry] | Agentes hospedados | Prepara un modelo, despliega un agente basado en el catálogo y conéctalo al sitio web |

## Requisitos previos

Antes de empezar este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo de **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] Permiso para crear un repositorio y un Codespace
- [ ] Conocimientos básicos del terminal o de la línea de comandos

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes verificados pueden obtener GitHub Copilot gratis a través de [GitHub Education][student-plan]. El plan **Copilot Student** incluye el agente, MCP, la revisión de código y las funciones de Copilot CLI que usa este taller.

> [!NOTE]
> Si usas Copilot Business o Copilot Enterprise, asegúrate de que tu administrador haya habilitado Copilot CLI.

## Primeros pasos

**[Empieza con los requisitos previos →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
