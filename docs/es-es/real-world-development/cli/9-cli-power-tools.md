---
title: "Lección 9 - Comandos de barra en GitHub Copilot CLI"
description: "Explora los comandos de barra para gestionar el contexto, seleccionar modelos, compartir sesiones y delegar tareas opcionalmente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier buena herramienta de CLI, GitHub Copilot CLI incluye muchos comandos de barra para interactuar con ella. Estos comandos exponen funciones avanzadas, información interna y opciones de configuración adicionales. Ya has usado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Vamos a explorar otros comandos útiles.

En esta lección, aprenderás a:

- usar `/context` y `/compact` para explorar cómo Copilot gestiona el contexto de la conversación.
- usar `/model` para explorar los modelos disponibles.
- aprender cómo `/share` puede exportar o compartir una sesión.
- explorar comandos opcionales para trabajar en paralelo, usar worktrees y delegar en un agente en la nube.

## Escenario

Ya has completado el flujo principal de CLI. Ahora veremos otras funciones: gestionar el contexto, cambiar de modelo, compartir sesiones y, de manera opcional, delegar trabajo en el [agente en la nube de Copilot][about-cloud-agent].

## Explora el contexto de Copilot CLI

Al trabajar en tareas más grandes o complejas, puede que alcances el límite de contexto del modelo. Copilot CLI compacta la conversación automáticamente cuando es necesario, y puedes inspeccionarla o compactarla por tu cuenta con comandos de barra.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio si aún no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y cómo se divide el contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio libre.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Escribe `/context` de nuevo y compara el resultado. Puede que no haya un cambio importante si la conversación ya es corta.

> [!NOTE]
> Copilot CLI compacta el contexto automáticamente cuando se llena la ventana. Usa `/compact` si quieres elegir el momento. Usa `/clear` o `/new` cuando cambies a una tarea no relacionada y prefieras empezar otra conversación.

## Elige el modelo

Los distintos modelos tienen fortalezas diferentes, y cada desarrollador tiene sus preferencias. Copilot CLI te permite ver los modelos disponibles y seleccionar el que quieras usar.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información de uso.
3. Mantén el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Comparte una sesión

Trabajar en equipo y compartir lo aprendido ayuda a todos a mejorar el uso de las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartir o publicar un gist de GitHub.

1. Escribe `/help` y revisa las opciones de `/share` en la versión instalada.
2. Si quieres compartir esta sesión, elige el destino que mejor se adapte a tus necesidades, por ejemplo, `/share file` para exportarla localmente como Markdown.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones de sesión pueden incluir prompts, respuestas y detalles del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio ni de la conversación que tu equipo no tenga intención de compartir.

## Opcional: amplía o delega

El taller principal ya está completo. Copilot CLI también ofrece comandos para tareas más grandes:

- `/fleet` puede dividir subtareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un entorno de trabajo de Git aislado en un worktree separado para otra tarea.
- `/delegate` puede enviar una tarea al agente en la nube de Copilot, que trabaja de forma asíncrona y puede abrir una pull request.

Estos comandos son opcionales porque pueden crear worktrees adicionales o trabajo remoto. Antes de probarlos, empieza con una tarea nueva y bien acotada, y revisa el resultado con tu flujo de trabajo habitual. Si quieres profundizar en el trabajo asíncrono con agentes, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y pasos siguientes

Los comandos de barra de Copilot CLI permiten configurarlo, compartir sesiones y ver lo que ocurre entre bastidores. En esta lección:

- usaste `/context` y `/compact` para explorar cómo Copilot gestiona el contexto de la conversación.
- usaste `/model` para explorar los modelos disponibles.
- aprendiste cómo `/share` puede exportar o compartir una sesión.
- exploraste comandos opcionales para trabajar en paralelo, usar worktrees y delegar en un agente en la nube.

Hay más comandos de barra y muchas otras cosas que explorar en Copilot CLI. Terminaremos este recorrido [repasando lo que hemos aprendido][next-lesson] y algunos pasos siguientes para continuar aprendiendo.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca del agente en la nube de Copilot][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
