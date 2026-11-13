---
title: "Lección 3 - Modos del agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia y el modo Interactivo para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos añadiendo una funcionalidad pequeña al proyecto. Sin embargo, los cambios más grandes requieren un proceso más sólido. Afortunadamente, GitHub Copilot CLI está diseñado para trabajar con el flujo existente de una organización y ayudar a construir las cosas correctas de la manera adecuada. Esta es la primera de varias lecciones en las que seguirás un proceso típico de desarrollo con agentes: usarás una incidencia para generar una nueva funcionalidad, comprobarás que el código es válido y que la funcionalidad se comporta como se espera y, por último, confirmarás que se integra correctamente en el proyecto.

> [!NOTE]
> Seguirás usando la misma conversación y la misma rama a medida que avances en el flujo de trabajo de la funcionalidad. Normalmente tendrías distintas ramas o PR para los distintos tipos de archivos en los que trabajarías, pero tomaremos un atajo para mantener el foco en los conceptos clave.

Para empezar, en esta lección aprenderás a:

- iniciar una nueva conversación de Copilot desde una incidencia de GitHub.
- definir requisitos en el modo Plan.
- implementar la nueva funcionalidad con el modo Autopilot.
- revisar el código.
- validar la funcionalidad manualmente en un navegador reenviado.

A medida que avances con esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad existente `quality-checks`, añadirás validación con MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo y los visitantes necesitan filtrar los juegos por categoría y editor. La incidencia del backlog describe la funcionalidad, pero es necesario acordar detalles como la combinación de categorías antes de escribir el código. Usarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Introducir agentes de programación con IA en tu flujo de trabajo no cambia los fundamentos. De hecho, ¡los hace aún más importantes! La mayoría de los desarrolladores siguen un flujo parecido a este:

1. Empezar con una incidencia registrada que describa el trabajo pendiente.
2. Crear un plan de lo que se debe construir.
3. Crear y revisar el código.
4. Ejecutar pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una pull request (PR).
7. Cuando se haya revisado el código y se haya completado correctamente la integración continua, combinar el código.

> [!NOTE]
> Los detalles exactos pueden variar según el equipo y la organización, pero la mayoría seguirá alguna variante de este proceso.

Si sigues este enfoque estándar, te aseguras de que el código generado por IA cumple los requisitos establecidos y pasa por el mismo proceso de revisión que el código escrito a mano.

## Modos de conversación

El **modo de conversación** controla el grado de autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar de modo:

- **Interactive**: Tú y el agente trabajáis juntos. El agente sugiere cambios y espera tu respuesta antes de continuar.
- **Plan**: El agente crea primero un plan y no puede editar los archivos del proyecto.
- **Autopilot**: El agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en el modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Empieza desde la incidencia

Antes de empezar a trabajar en el filtrado, vuelve a tu Codespace y comprueba que el repositorio y el terminal estén listos.

1. Vuelve a tu Codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que la PR de valoración por estrellas está combinada.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y, a continuación, crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las flechas para resaltar la incidencia titulada **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

Observa que el prompt ahora empieza con `#7` (o un número similar). El carácter `#` permite incluir una incidencia o una pull request (PR) de GitHub en el contexto.

## Planifica la funcionalidad de filtrado

La planificación te permite definir cómo se implementará una funcionalidad o se realizará una tarea antes de asignársela a Copilot. Siempre es buena idea dedicar tiempo a planificar las tareas complejas. Cambiaremos al modo Plan y pediremos a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador debajo del prompt muestra **Plan**.
2. Después de la referencia a la incidencia que añadiste en el paso anterior, escribe el siguiente prompt:

   ```plaintext
   Crea un plan para implementar esta funcionalidad.
   ```

Copilot empezará a preparar el plan. Primero explorará el proyecto y, después, determinará el mejor enfoque.

3. Durante el proceso, Copilot puede preguntarte cómo deberían funcionar las capacidades de filtrado. Responde según tus preferencias. ¡No hay respuestas incorrectas!
4. Cuando el plan esté completo, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Desplázate hacia arriba y hacia abajo para revisar el plan.
6. Pide a Copilot que revise cualquier parte del plan que no coincida con tus decisiones.

## Aprueba Autopilot

Una vez redactado y revisado el plan, ¡es el momento de implementarlo! Deja que Copilot trabaje en modo Autopilot.

Autopilot permite que Copilot itere en la tarea hasta que considere que está completa.

1. Selecciona **Accept plan and build on autopilot (recommended)** o una opción con un texto similar en la versión instalada.
2. Confirma que el indicador debajo del prompt muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación de inmediato, así que revisa primero el plan. Si Copilot informa de dependencias faltantes o de un conflicto de puertos, resuelve el problema de configuración antes de considerar que las comprobaciones están completas.

## Revisa y verifica la implementación

Una vez generado el código, debes revisarlo antes de combinarlo, igual que cualquier otro código. Revisaremos el código y ejecutaremos el sitio para comprobar que todo funciona correctamente.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar en el modo Interactive. Confirma que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Introduce `/diff` e inspecciona la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y las decisiones que tomaste durante la planificación.
4. Cuando hayas revisado el código, pulsa <kbd>Esc</kbd> para salir de la vista del diff.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que corrija cualquier error.

## Explora la nueva funcionalidad

El código parece estar bien, pero ¿se ejecuta? Iniciaremos la aplicación como antes y abriremos el sitio mediante el puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Inicia la aplicación para que pueda probar la funcionalidad de filtrado en mi navegador. Dime la URL y deja el servidor en ejecución.
   ```

2. Cuando Codespaces informe de que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba el filtrado por categoría, el filtrado por editor y las combinaciones acordadas en el plan.
4. Confirma que el comportamiento de restablecimiento y de resultados vacíos coincide con la incidencia y con tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo que inició.

## Resumen y pasos siguientes

Has usado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección:

- iniciaste una nueva conversación de Copilot desde una incidencia de GitHub.
- definiste requisitos en el modo Plan.
- implementaste la nueva funcionalidad con el modo Autopilot.
- revisaste el código.
- validaste manualmente la funcionalidad en un navegador reenviado.

A continuación, profundizarás en cómo se genera el código y comprobarás que sigue prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
