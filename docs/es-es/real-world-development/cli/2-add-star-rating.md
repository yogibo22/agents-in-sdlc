---
title: "Lección 2 - Añade valoraciones por estrellas: una mejora rápida"
description: "Usa Copilot CLI para hacer un cambio pequeño en las fichas de juegos, revísalo en un navegador reenviado y fusiona tu primera pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ahora que ya has instalado Copilot CLI y has probado una conversación, es el momento de hacer tu primer cambio en el proyecto. Mantendremos el alcance pequeño: los juegos ya tienen una valoración por estrellas en sus datos, pero las fichas de juego en la página de inicio aún no la muestran. Pedirás al agente que la muestre, revisarás el cambio y lo fusionarás como tu primera pull request.

En esta lección, aprenderás a:

- iniciar una conversación enfocada de Copilot en una rama de funcionalidad.
- pedir al agente que haga un cambio pequeño en el proyecto.
- revisar el cambio con `/diff`.
- ejecutar la aplicación para confirmar el cambio en un navegador reenviado.
- abrir y fusionar tu primera pull request.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración por estrellas, y ya aparece en la página de detalles del juego. Sin embargo, las fichas de juego en la página de inicio solo muestran el título, la categoría, el editor y la descripción. Como calentamiento, pedirás al agente que muestre la valoración existente en cada ficha: un cambio pequeño y autocontenido que es perfecto para tu primera sesión.

## Anatomía de una conversación

Una **conversación** es el lugar donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de CLI usa el repositorio y la rama de Git actualmente comprobados en tu terminal, en lugar de crear un árbol de trabajo dedicado. Las conversaciones guardadas te permiten volver a la misma discusión más adelante, mientras que los archivos y la rama siguen siendo estado Git normal en disco.

Dentro de una conversación verás tres cosas: tus mensajes y las respuestas del agente, la actividad de herramientas del agente mientras explora y edita archivos, y los cambios que puedes inspeccionar con `/diff`.

## Inicia una conversación y solicita nuestro cambio

Vamos a comenzar una nueva conversación para implementar nuestra funcionalidad.

1. Vuelve a tu Codespace.
2. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Si aún no está abierto, inicia Copilot con el siguiente comando:

   ```bash
   copilot --yolo
   ```

4. Asegúrate de que se inicia una nueva sesión con el comando slash `/new` y pulsa <kbd>Enter</kbd>.
5. Usa el siguiente mensaje para solicitar el cambio:

   ```plaintext
   Muestra la starRating de cada juego sobre 5 en las fichas del listado. Si la valoración es nula, muestra "No rating yet". Mantén el diseño de la ficha como está, añade pruebas y ejecuta las comprobaciones relevantes.
   ```

Copilot explora el proyecto, localiza los archivos que muestran los detalles del juego y crea el código necesario. ¡Ya has añadido una nueva funcionalidad con Copilot CLI!

## Revisa el diff

Todos los cambios generados por IA merecen una revisión antes de fusionarse, incluso los pequeños. Vamos a explorar los cambios directamente aquí en Copilot CLI.

1. Introduce `/diff` e inspecciona cada archivo modificado.
2. Confirma que la ficha del juego muestra la valoración numérica cuando existe y `No rating yet` cuando `starRating` es `null`.
3. Confirma que las pruebas cubren ambos estados.
4. Revisa los resultados de las comprobaciones que Copilot ejecutó y pídele que corrija cualquier error.
5. Una vez finalices tu revisión, pulsa <kbd>Esc</kbd> para salir de la vista del diff.

> [!NOTE]
> Como Copilot, como todas las herramientas generativas de IA, es probabilístico y no determinista, tu código exacto puede variar. Revisa el comportamiento en lugar de esperar una implementación exacta.

## Comprueba los cambios

Claro que no deberíamos leernos el código y asumir que funciona. Vamos a pedirle a Copilot que inicie nuestra web para revisar la interfaz de usuario actualizada en el navegador.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Inicia la aplicación para que pueda inspeccionar el cambio de valoración por estrellas en mi navegador. Dime la URL y deja el servidor en ejecución.
   ```

2. Cuando Codespaces informe que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las fichas muestran sus valoraciones sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que inició:

   ```plaintext
   Detén el servidor de desarrollo que has iniciado.
   ```

## Abre y fusiona tu primera pull request

¡Ya has creado la funcionalidad! Es el momento de crear una pull request (PR) para fusionar el nuevo código en el proyecto.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Confirma los cambios revisados de valoración por estrellas con un mensaje de confirmación apropiado.
   ```

2. Introduce `/pr create`. Copilot CLI empuja el commit existente cuando crea la PR y muestra la URL de la PR.
3. Abre la PR manteniendo pulsado <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la URL mostrada por Copilot CLI.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando estés listo, selecciona **Merge pull request** y confirma la fusión.
6. Vuelve a tu Codespace y sal de Copilot CLI con `/exit`.
7. Actualiza tu `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y siguientes pasos

¡Enhorabuena! ¡Has enviado tu primer cambio usando GitHub Copilot CLI! En concreto, has:

- iniciado una conversación enfocada de Copilot en una rama de funcionalidad.
- indicado al agente que hiciera un cambio pequeño en las fichas de juegos.
- revisado el cambio con `/diff`.
- ejecutado la aplicación para confirmar la valoración por estrellas en un navegador reenviado.
- abierto y fusionado tu primera pull request.

A continuación, empezarás desde la incidencia de filtrado y usarás los modos Plan y Autopilot [para construir una funcionalidad más grande][next-lesson].

## Recursos

- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
