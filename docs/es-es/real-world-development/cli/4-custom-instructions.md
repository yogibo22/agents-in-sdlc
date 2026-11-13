---
title: "Lección 4 - Guiar a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, conviene que Copilot tenga disponible esa guía. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que ya has creado el filtrado, explorarás las instrucciones que usó Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección, aprenderás a:

- explorar cómo las instrucciones del repositorio y los archivos de instrucciones por ruta llegan al agente.
- actualizar el archivo de instrucciones para asegurar que se siguen los estándares de codificación.
- ver el impacto de los archivos de instrucciones en el código.

## Escenario

Como cualquier buen taller de desarrollo, Tailspin Toys tiene un conjunto de directrices y requisitos para las prácticas de desarrollo. Entre ellos:

- Los comentarios deben explicar la intención y las decisiones no obvias en lugar de repetir el código.
- Las funciones exportadas en `db/` y `src/lib/` deben documentar su propósito, parámetros y valores de retorno con TSDoc/JSDoc, incluido el argumento inyectable `db` cuando exista.
- Los componentes reutilizables de Astro deben documentar sus contratos de `Props`, y los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Se deben conservar las directrices de formato y lint existentes.

Al usar archivos de instrucciones, te asegurarás de que Copilot disponga de la información adecuada para realizar las tareas de acuerdo con las prácticas destacadas.

## Archivos de instrucciones

Las instrucciones personalizadas te permiten proporcionar contexto y preferencias a Copilot para que comprenda mejor tu estilo de codificación y tus requisitos. Es una función potente que te ayuda a obtener sugerencias y fragmentos de código más relevantes. Puedes especificar tus convenciones de codificación preferidas, bibliotecas e incluso los tipos de comentarios que quieres incluir en el código. Puedes crear instrucciones para todo el repositorio o para tipos específicos de archivos con contexto a nivel de tarea.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo de instrucciones que se envía a Copilot para **todas** las solicitudes del repositorio. Debe contener información a nivel de proyecto pertinente para la mayoría de las solicitudes.
- Archivos `.github/instructions/*.instructions.md`, que proporcionan directrices para lenguajes, tipos de archivo o tareas concretas.

> [!NOTE]
> La compatibilidad con otros formatos de instrucciones varía según el entorno. Consulta la [referencia de compatibilidad de instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explora los archivos de instrucciones personalizadas de este proyecto

El proyecto inicial ya incluye un conjunto de archivos de instrucciones. Antes de hacer cambios, exploremos qué contienen para ver su impacto.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces (no en el terminal), abre `.github/copilot-instructions.md`.
3. Explora el archivo y fíjate en la breve descripción del proyecto y las directrices de codificación. Estas instrucciones se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y explora los archivos. Verás instrucciones para archivos de Astro, la capa de datos de Drizzle, pruebas y más.
5. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` de la parte superior: define un patrón glob que determina a qué archivos se aplican estas instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y observa sus enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así se pueden dividir conjuntos grandes de instrucciones en archivos más pequeños y reutilizables, y señalar a Copilot ejemplos que debe seguir.

## Actualiza los archivos de instrucciones para que coincidan con las directrices del equipo

Aunque los archivos existentes son un buen punto de partida, falta una indicación. Modificaremos el archivo central `copilot-instructions.md` para asegurar que el nuevo código TypeScript incluya comentarios TSDoc.

1. En `.github/copilot-instructions.md`, localiza la sección **Code formatting guidance**, que debería estar alrededor de la línea 35.
2. Añade el siguiente elemento al final de la lista:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Usa las directrices actualizadas

Copilot CLI carga las instrucciones del repositorio cuando se inicia una conversación. Después de guardar las instrucciones actualizadas, cierra y reanuda la conversación de filtrado para que Copilot empiece con las nuevas directrices sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit` para cerrarla. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio, en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te pide, selecciona la conversación de filtrado y confirma que está seleccionada la rama `game-filters-cli`.
3. Pide a Copilot que aplique las directrices actualizadas:

   ```plaintext
   Acabamos de actualizar nuestras instrucciones y las directrices de código. ¿Puedes actualizar el código que generaste para que las siga?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Observa los nuevos comentarios TSDoc generados y confirma que explican el código con precisión.

## Resumen y pasos siguientes

Has explorado cómo Copilot CLI recoge el contexto de los archivos de instrucciones y has aplicado un nuevo estándar a tu funcionalidad. En concreto:

- exploraste cómo las instrucciones del repositorio y los archivos de instrucciones por ruta llegan al agente.
- actualizaste el archivo de instrucciones para asegurar que se siguen los estándares de codificación.
- viste el impacto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable `quality-checks`][next-lesson] para asegurar que se ejecuten el linting y las pruebas.

## Recursos

- [Añadir instrucciones personalizadas para Copilot CLI][instruction-files]
- [Compatibilidad de instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
