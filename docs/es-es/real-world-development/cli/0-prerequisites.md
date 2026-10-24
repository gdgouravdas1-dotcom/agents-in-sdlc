---
title: "Lección 0: requisitos previos"
description: "Crea tu copia de Tailspin Toys y prepara un codespace para el taller de Copilot CLI."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Antes de empezar las lecciones de Copilot CLI, prepara todo lo necesario. Crearás tu copia del repositorio Tailspin Toys y abrirás un [codespace][codespaces]. En la siguiente lección usarás su terminal integrado para instalar y ejecutar Copilot CLI.

En esta lección:

- crearás tu propia copia del proyecto Tailspin Toys a partir de la plantilla;
- crearás un codespace y confirmarás que el proyecto está listo.

## Configurar el repositorio del laboratorio

Trabajarás con tu propia copia del proyecto Tailspin Toys. Créala a partir del [repositorio de plantilla][tailspin-template]. El nuevo repositorio incluirá todos los archivos necesarios para el laboratorio.

1. En una ventana nueva del navegador, abre la [plantilla de Tailspin Toys][tailspin-template].
2. Crea tu copia del repositorio seleccionando **Use this template** y, después, **Create a new repository**.
3. Si participas en un evento dirigido por GitHub o Microsoft, sigue las instrucciones de los mentores. De lo contrario, crea el repositorio en una organización donde tengas acceso a GitHub Copilot.
4. Anota la ruta del repositorio que has creado (`nombre-de-organización-o-usuario/nombre-del-repositorio`); la necesitarás durante el taller.

> [!NOTE]
> Al crear el repositorio a partir de la plantilla, se crea automáticamente un backlog de incidencias de GitHub. Trabajarás con esas incidencias durante el taller; no tienes que crear ninguna.

Usa una copia nueva de la plantilla del taller. Incluye las instrucciones del repositorio, el código de la aplicación, las pruebas, la habilidad `quality-checks` y el backlog que necesitas. Si usas una copia antigua, confirma con el facilitador que contiene los archivos necesarios.

## Crear un codespace

Usarás un codespace para completar el taller. [GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que permite escribir, ejecutar y depurar código directamente en el navegador. Proporciona un editor completo compatible con distintos lenguajes, extensiones y herramientas.

1. Abre el repositorio que acabas de crear.
2. Selecciona **Code**.
3. Selecciona la pestaña **Codespaces** y, después, **Create codespace on main**.
4. Espera a que termine la configuración. La plantilla instala las dependencias del proyecto, Chromium de Playwright y la base de datos local.
5. Si aparece el mensaje **Do you trust the authors of the files in this folder?**, selecciona **Trust Folder & Continue**.
6. Abre un terminal en la raíz del repositorio e inicia la aplicación:

   ```bash
   npm run dev
   ```

7. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser** y confirma que se carga el sitio de Tailspin Toys.
8. Vuelve al terminal y detén el servidor de desarrollo con <kbd>Ctrl</kbd>+<kbd>C</kbd>.

## Resumen y siguientes pasos

Ya tienes todo preparado. En esta lección:

- has creado tu copia del proyecto Tailspin Toys a partir de la plantilla;
- has creado un codespace y confirmado que el proyecto estaba listo.

A continuación, [instalarás GitHub Copilot CLI][next-lesson] en tu codespace y lo autenticarás con tu cuenta de GitHub.

## Recursos

- [Información general sobre GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Primeros pasos con Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
