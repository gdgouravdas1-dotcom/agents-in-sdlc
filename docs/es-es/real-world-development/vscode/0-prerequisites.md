---
title: "Ejercicio 0: requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de empezar los ejercicios de VS Code, prepara todo lo necesario. Crearás tu copia del repositorio Tailspin Toys, abrirás un [codespace][codespaces] y confirmarás que GitHub Copilot Chat funciona en el editor.

## Configurar el repositorio del laboratorio

Para crear una copia del repositorio donde escribirás código, crearás una instancia a partir de la [plantilla][template-repository]. La nueva instancia contendrá los archivos necesarios para el laboratorio.

1. En una ventana nueva del navegador, abre la [plantilla de Tailspin Toys][tailspin-template].
2. En la página del repositorio del laboratorio, selecciona **Use this template** y, después, **Create a new repository**.

    ![Botón Use this template](../../../_images/ex0-use-template.png)

3. Si participas en un evento dirigido por GitHub o Microsoft, sigue las instrucciones de los mentores. De lo contrario, crea el repositorio en una organización donde tengas acceso a GitHub Copilot.

    ![Configuración de la plantilla del repositorio](../../../_images/ex0-repository-settings.png)

4. Anota la ruta del repositorio que has creado (**nombre-de-organización-o-usuario/nombre-del-repositorio**); la necesitarás más adelante.

> [!NOTE]
> **El backlog está listo**
>
> Al crear el repositorio a partir de la plantilla, se crea automáticamente un backlog de incidencias de GitHub. Trabajarás con esas incidencias durante el taller; no tienes que crear ninguna.

## Crear un codespace

Usarás un codespace para completar los ejercicios. [GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que permite escribir, ejecutar y depurar código en el navegador. Proporciona un IDE completo con compatibilidad para varios lenguajes, extensiones y herramientas.

1. Abre el repositorio nuevo.
2. Selecciona el botón verde **Code**.

    ![Seleccionar el botón Code](../../../_images/ex0-code-button.png)

3. Selecciona la pestaña **Codespaces** y, después, el botón **+** para crear un codespace.

    ![Crear un codespace](../../../_images/ex0-create-codespace.png)

La creación del codespace puede tardar varios minutos, aunque será mucho más rápida que instalar manualmente los servicios. Mientras esperas, puedes explorar otras funciones de GitHub Copilot.

> [!CAUTION]
> Volverás al codespace en un ejercicio posterior. Por ahora, déjalo abierto en una pestaña del navegador.

> [!NOTE]
> Este taller está diseñado para ejecutarse en un codespace o en un [contenedor de desarrollo][dev-containers] local, que incluyen los requisitos previos necesarios. Para trabajar localmente, abre el repositorio clonado en VS Code y selecciona **Reopen in Container** cuando aparezca el mensaje. VS Code creará el mismo contenedor que usa el codespace.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Usar GitHub Copilot Chat y el modo agente

Para usar el modo agente de GitHub Copilot Chat, necesitas la extensión GitHub Copilot Chat en el IDE. Ya debería estar instalada si utilizas GitHub Codespaces.

> [!TIP]
> Si no tienes la extensión, [instálala desde Visual Studio Code Marketplace][copilot-chat-extension] o abre la vista **Extensions** de Visual Studio Code, busca **GitHub Copilot Chat** y selecciona **Install**.

Una vez instalada, puede que tengas que autenticarte con tu cuenta de GitHub.

1. Vuelve al codespace.
2. Si Copilot Chat no aparece a la derecha del editor, selecciona el icono **Copilot Chat** en la parte superior del codespace.
3. Escribe un mensaje como «Hello world» en la ventana de Copilot Chat y pulsa <kbd>Enter</kbd>.
4. Si no te has autenticado, sigue las instrucciones para iniciar sesión en tu cuenta de GitHub.

    ![Mensaje de autenticación de Copilot Chat](../../../_images/ex1-copilot-authentication.png)

5. Cuando termines, verás la ventana de Copilot Chat.

## Resumen

Has creado una copia del repositorio del laboratorio e iniciado un codespace para escribir código.

## Siguiente paso

Continúa con el [Ejercicio 1: instrucciones personalizadas][next-lesson] para enseñar a Copilot las convenciones del proyecto.

## Recursos

- [Información general sobre GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Primeros pasos con Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
