---
title: "Lección 12 - Publicar el sitio con GitHub Pages"
description: "Usa la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su solicitud de incorporación de cambios y comprobar el Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Publica el Space Quiz en GitHub Pages para que todo el mundo pueda acceder a él. Copilot puede preparar el flujo de trabajo y guiarte durante la publicación, pero debes revisar los cambios y confirmar que el sitio está disponible.

En esta lección:

- Pide a Copilot que configure la publicación en GitHub Pages.
- Revisa el flujo de trabajo y la solicitud de incorporación de cambios antes de combinarla.
- Sigue la publicación y comprueba el sitio publicado.

## Escenario

El Space Quiz ya está en un repositorio de GitHub, pero todavía hace falta una forma sencilla de probarlo. Publicar el sitio de un solo archivo con GitHub Pages permite acceder a él desde una URL pública sin añadir un servidor ni dependencias de ejecución.

## Pide a Copilot que publique el sitio

En la sesión de la aplicación Copilot para `space-quiz`, envía este prompt:

```plaintext
Habilita GitHub Pages para este repositorio. Crea un flujo de trabajo de GitHub Actions que compile el sitio web si es necesario y lo publique en GitHub Pages. Añade un README que describa la aplicación y cómo ejecutarla localmente, si todavía no existe. Crea una solicitud de incorporación de cambios con estos cambios y combínala cuando se superen las comprobaciones obligatorias. Supervisa la publicación hasta que se complete correctamente y, después, indica la URL final del sitio web activo. Si no tienes permiso para habilitar Pages o combinar la solicitud, dime exactamente qué debo aprobar o hacer; no indiques que se ha completado correctamente hasta que el sitio esté publicado.
```

![Diagrama de tres etapas: una solicitud de incorporación de cambios de space-quiz combinada; una ejecución correcta de Deploy website en GitHub Actions con los pasos Build website, Upload artifact y Deploy to Pages; y el Space Quiz publicado en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Este prompt pide al agente que cree y combine una solicitud de incorporación de cambios y cambie la configuración de Pages del repositorio. Antes de continuar, revisa el diff, los permisos de publicación y cualquier solicitud de aprobación de GitHub.

## Revisa el flujo de trabajo y comprueba la publicación

1. Confirma que el README solo se ha añadido si el repositorio todavía no tenía uno.
2. Revisa los cambios de la solicitud de incorporación de cambios, incluidos los pasos de compilación y publicación del flujo de trabajo.
3. Combina la solicitud solo después de que se superen las comprobaciones obligatorias.
4. Sigue la ejecución de publicación de GitHub Actions hasta que se complete correctamente.
5. Abre la URL indicada y confirma que se carga el Space Quiz.

Si el agente no puede habilitar Pages o combinar la solicitud, sigue las instrucciones concretas que te proporcione. No des la tarea por terminada hasta que la publicación se complete correctamente y el sitio esté disponible.

## Resumen y pasos siguientes

El Space Quiz ya está publicado y has comprobado la publicación en lugar de confiar en el informe del agente. Continúa con la [Lección 13: Repaso y pasos siguientes][next-lesson].

[next-lesson]: ../13-review/
