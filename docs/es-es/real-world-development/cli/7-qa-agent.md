---
title: "Lección 7 - Crear y usar un agente de QA"
description: "Crea un agente personalizado de QA que combine la incidencia, la habilidad quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ya has usado la habilidad `quality-checks` para ejecutar comprobaciones automáticas y Playwright MCP para observar la experiencia de filtrado en el navegador. Ahora reunirás esas capacidades en un agente personalizado con un proceso de QA claramente definido.

En esta lección, aprenderás a:

- explorar cómo funciona un agente personalizado con instrucciones, habilidades y herramientas MCP.
- crear e inspeccionar un perfil reutilizable de aseguramiento de calidad.
- seleccionar el agente de QA y revisar sus hallazgos frente a la incidencia de filtrado.

## Escenario

Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automáticas, la cobertura de pruebas y el comportamiento del navegador antes de abrir una pull request (PR). Un agente personalizado puede coordinar ese proceso de QA y proporcionar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida en un perfil de Markdown. El perfil describe el propósito del agente, sus instrucciones y las herramientas disponibles. En este taller, definirás un rol de QA en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Las personalizaciones que ya has usado tienen funciones distintas. Las instrucciones del repositorio describen los estándares del equipo. La habilidad `quality-checks` agrupa comprobaciones repetibles. Playwright MCP proporciona herramientas del navegador. El perfil de QA indica a Copilot cómo usar esas capacidades para evaluar los requisitos e informar de los hallazgos. No sustituye a esas herramientas ni requiere otra conversación.

## Crea el perfil de QA

Antes de abrir la PR de la funcionalidad, pedirás a Copilot que cree un perfil de QA reutilizable. El perfil definirá tanto las comprobaciones de QA como los límites que debe respetar.

1. Vuelve a tu Codespace y confirma que la conversación de filtrado está en el modo Interactive.
2. Pide al agente predeterminado que cree el nuevo agente personalizado:

   ```plaintext
   Crea un agente personalizado llamado QA en .github/agents/qa.agent.md. Debe comprobar las funcionalidades frente a sus incidencias y los requisitos acordados, seguir las instrucciones del repositorio, ejecutar la habilidad quality-checks, usar Playwright MCP para verificar el comportamiento y añadir pruebas si falta cobertura.

   Debe indicar cada requisito como aprobado, fallido o bloqueado, con pruebas que respalden el resultado. Debe preguntar antes de cambiar el código de implementación y no debe confirmar cambios ni abrir pull requests.

   Por ahora, crea solo el perfil para que pueda revisarlo.
   ```

## Inspecciona el perfil

Antes de usar el nuevo agente, revisa su perfil para confirmar que Copilot ha capturado el flujo de trabajo y los límites previstos de QA. Así evitarás que un agente incompleto o demasiado amplio modifique la funcionalidad cuando solo quieres verificarla.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee el frontmatter. `description` es obligatorio; `name` es opcional, pero incluirlo da al agente un nombre visible claro.
3. Lee las instrucciones del perfil y confirma que QA parte de los requisitos, sigue las instrucciones del repositorio, ejecuta la habilidad `quality-checks` y usa Playwright MCP.
4. Confirma que QA informa de sus hallazgos con pruebas, pregunta antes de cambiar el código de implementación y no confirma cambios ni abre pull requests.
5. Si el perfil generado no incluye alguna de estas responsabilidades o límites, pide al agente predeterminado que lo revise antes de continuar.

## Ejecuta QA frente a la incidencia

Copilot CLI carga los agentes del proyecto cuando se inicia una conversación. Después de crear el perfil, reanuda la misma conversación de filtrado y selecciona QA para que pueda usar la incidencia y las decisiones de planificación que ya forman parte del contexto.

1. Activa el agente con el siguiente prompt:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, es posible que no aparezca en la lista de agentes disponibles. Está ahí y el comando anterior lo activará.

2. Usa el siguiente prompt para pedir al agente de QA que revise la funcionalidad:

   ```plaintext
   Revisa la funcionalidad de filtrado según la incidencia y las decisiones de nuestro plan. ¿Está lista para una PR?
   ```

3. ¡El agente de QA empezará a trabajar!
4. Lee el informe que te proporcione cuando termine.

## Resumen y pasos siguientes

Has añadido un rol especialista reutilizable al flujo de trabajo y has revisado su trabajo. En esta lección:

- exploraste cómo funciona un agente personalizado con instrucciones, habilidades y herramientas MCP.
- creaste e inspeccionaste un perfil reutilizable de aseguramiento de calidad.
- seleccionaste el agente de QA y revisaste sus hallazgos frente a la incidencia de filtrado.

Ya tienes la implementación, la actualización de la habilidad, el perfil de QA, las pruebas y el informe de verificación listos para revisión. A continuación, [los reunirás en una PR de la funcionalidad y usarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
