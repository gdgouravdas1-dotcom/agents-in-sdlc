---
title: "Lección 12: publicar el sitio con GitHub Pages"
description: "Usa la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su solicitud de incorporación de cambios y verificar el Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Publica Space Quiz con GitHub Pages para que tenga una dirección accesible al público. Copilot puede preparar el flujo de trabajo y guiar la implementación, pero tú debes revisar los cambios y confirmar que el sitio está publicado.

En esta lección:

- pedirás a Copilot que configure la publicación con GitHub Pages;
- revisarás el flujo de trabajo y la solicitud de incorporación de cambios antes de combinarla;
- seguirás la implementación y comprobarás el sitio publicado.

## Escenario

Space Quiz ya está en un repositorio de GitHub, pero todavía no es fácil probarlo. Publicar el sitio de un solo archivo con GitHub Pages lo hace accesible desde una URL pública sin añadir un servidor ni dependencias de ejecución.

## Pedir a Copilot que publique el sitio

En la sesión de la aplicación Copilot para `space-quiz`, envía esta indicación:

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Diagrama de tres etapas: solicitud de incorporación de cambios combinada para space-quiz; ejecución correcta de Deploy website en GitHub Actions, con los pasos Build website, Upload artifact y Deploy to Pages; y Space Quiz publicado en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Esta indicación pide al agente que cree y combine una solicitud de incorporación de cambios y cambie la configuración de Pages del repositorio. Revisa la diferencia, los permisos de implementación y cualquier solicitud de aprobación de GitHub antes de continuar.

## Revisar el flujo de trabajo y verificar la implementación

1. Comprueba que el README solo se haya añadido si el repositorio aún no tenía uno.
2. Revisa los cambios de la solicitud de incorporación de cambios, incluidos los pasos de compilación e implementación del flujo de trabajo.
3. Combínala solo cuando se hayan superado las comprobaciones obligatorias.
4. Sigue la ejecución de implementación de GitHub Actions hasta que termine correctamente.
5. Abre la URL indicada y comprueba que Space Quiz se carga.

Si el agente no puede habilitar Pages o combinar la solicitud, sigue las instrucciones específicas que te proporcione. No des la tarea por terminada hasta que la implementación haya finalizado y el sitio publicado se cargue.

## Resumen y próximos pasos

Space Quiz ya está publicado y has verificado la implementación en lugar de confiar únicamente en el informe del agente. Continúa con la [Lección 13: repaso y próximos pasos][next-lesson].

[next-lesson]: ../13-review/
