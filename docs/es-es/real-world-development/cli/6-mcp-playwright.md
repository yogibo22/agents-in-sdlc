---
title: "Lección 6 - Validar la funcionalidad con Playwright MCP"
description: "Inspecciona o configura Playwright MCP en Copilot CLI y úsalo para explorar la funcionalidad de filtrado en un navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos destacado, escribir código no consiste solo en escribir código. También tenemos que trabajar con datos, servicios externos e incluso permitir automatizaciones adicionales para Copilot. Aquí es donde entran los servidores MCP. Permiten que Copilot vaya más allá de lo que ya incluye la CLI y le proporcionan más herramientas y servicios.

En esta lección, aprenderás a:

- comprender qué es Model Context Protocol (MCP) y cómo lo usa Copilot CLI.
- añadir el servidor Playwright MCP si aún no está disponible.
- pedir al agente que controle un navegador y explore la funcionalidad de filtrado.

## Escenario

Aunque las pruebas unitarias y de extremo a extremo son importantes, validar las actualizaciones de la interfaz requiere interactuar con ella. Quieres que Copilot use el sitio web en el que estás trabajando como lo haría un usuario para automatizar aún más los cambios y aumentar la confianza en que las actualizaciones funcionan como se espera.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] ofrece a los agentes de IA una forma de comunicarse con herramientas y servicios externos. Al usar MCP, los agentes de IA pueden comunicarse con herramientas y servicios externos en tiempo real. Esto les permite acceder a información actualizada y realizar acciones en tu nombre.

Se accede a estas herramientas y recursos a través de un servidor MCP, que actúa como puente entre el agente de IA y las herramientas y servicios externos. Cada servidor MCP representa un conjunto diferente de herramientas y recursos a los que puede acceder el agente de IA.

Algunos servidores MCP populares son:

- **[GitHub MCP Server][github-mcp]**: proporciona acceso a API para gestionar repositorios, incidencias y pull requests de GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: proporciona funciones de automatización del navegador mediante Playwright.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro de MCP][mcp-registry] para mejorar la visibilidad y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usarlos, revisa su código fuente, comprueba quién los publica y considera las implicaciones de seguridad.

## Añade el servidor Playwright MCP

Si Playwright MCP aún no está disponible, añádelo para que Copilot pueda interactuar con el sitio web como lo haría un usuario.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos de configuración y continúa con la siguiente sección.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo para añadir un servidor MCP con el siguiente comando en Copilot CLI:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Confirma que el tipo de servidor es **STDIO**; para ello, pulsa <kbd>Enter</kbd> y luego <kbd>Tab</kbd>.
5. Pega lo siguiente en el cuadro de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el nuevo servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo MCP.

## Pide a Copilot que explore la funcionalidad con Playwright

Antes comprobaste manualmente que la funcionalidad se comportaba como se esperaba. Ahora usaremos el servidor Playwright MCP disponible en Copilot para hacer lo mismo.

1. Usa el siguiente prompt para pedir a Copilot que valide la funcionalidad con Playwright MCP:

   ```plaintext
   Inicia la aplicación y usa Playwright MCP para comprobar el filtrado según la incidencia y nuestro plan. Dime qué funciona y qué no, sin hacer cambios. Cuando termines, detén el servidor que hayas iniciado.
   ```

> [!NOTE]
> No es necesario pedirle a Copilot que use el servidor MCP, porque normalmente lo descubrirá por sí solo. Sin embargo, como sabes qué debe usar, puedes orientarlo para obtener resultados más coherentes y ahorrar algunos tokens.

2. Observa cómo Copilot enumera los distintos pasos que realiza en el navegador para comprobar que la funcionalidad funciona.
3. Lee el informe y confirma que todo se comporta como se esperaba.

Copilot iniciará el servidor, usará Playwright para interactuar con el sitio web, detendrá el servidor y te proporcionará un informe.

## Resumen y pasos siguientes

¡Enhorabuena! Has usado el servidor Playwright MCP para explorar la funcionalidad en un navegador real desde Copilot CLI. En resumen:

- aprendiste qué es Model Context Protocol (MCP) y cómo lo usa Copilot CLI.
- añadiste el servidor Playwright MCP si aún no estaba disponible.
- pediste al agente que controlara un navegador y explorara la funcionalidad de filtrado.

A continuación, [crearás un agente personalizado de QA][next-lesson] que combinará la habilidad y las herramientas del navegador en un rol especializado.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
