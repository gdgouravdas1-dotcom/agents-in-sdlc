---
title: "Lección 1: instalar GitHub Copilot CLI"
description: "Instala y autentica Copilot CLI en tu codespace, familiarízate con la herramienta y localiza la incidencia de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] es un potente asistente de programación con agentes que se ejecuta en el terminal. Permite explorar bases de código, generar código, ejecutar comandos e interactuar con herramientas externas, todo desde la línea de comandos. Puedes delegar tareas, pedir cambios y mantener la concentración. El primer paso es instalar la herramienta, algo que puedes hacer con herramientas que ya conoces.

En esta lección:

- instalarás GitHub Copilot CLI con npm;
- te autenticarás con tu cuenta de GitHub;
- confiarás en el repositorio del taller y probarás una conversación;
- localizarás la incidencia de filtrado con el servidor MCP integrado de GitHub.

## Escenario

Tu equipo empieza a usar agentes de IA para gestionar un backlog cada vez mayor. Copilot CLI lleva esa capacidad al terminal, donde muchos desarrolladores ya trabajan. En esta lección instalarás y autenticarás la herramienta para usarla durante el resto del taller.

## Instalar Copilot CLI

Puedes instalar Copilot CLI mediante [npm][install-cli], WinGet o Homebrew. Como GitHub Codespaces ya incluye Node.js, usarás npm.

1. Vuelve al codespace y abre un terminal.
2. Comprueba que Node.js está instalado y cumple el requisito de versión:

   ```bash
   node --version
   ```

   Deberías ver la versión 24 o posterior.

3. Instala Copilot CLI de forma global:

   ```bash
   npm install -g @github/copilot
   ```

4. Comprueba la instalación:

   ```bash
   copilot --version
   ```

   Deberías ver el número de versión.

## Autenticarse con GitHub

La primera vez que inicies Copilot CLI, te pedirá que te autentiques con tu cuenta de GitHub.

1. Inicia Copilot CLI:

   ```bash
   copilot
   ```

2. Si se te solicita, sigue las instrucciones del código de dispositivo para autenticar y autorizar Copilot CLI.
3. Copilot CLI muestra este mensaje:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Comprueba que la ruta sea la del repositorio Tailspin Toys y selecciona **Yes, and remember this folder for future sessions**.

> [!NOTE]
> En un codespace puede que ya estés autenticado con tu sesión de GitHub. Si Copilot CLI se inicia sin pedir autenticación, ya está listo.

## Familiarizarse con Copilot CLI

Los comandos del terminal normal se ejecutan directamente en el codespace. Una vez iniciado Copilot CLI, el lenguaje natural se envía al agente y los comandos de barra controlan la conversación.

1. Escribe `/model`, usa las flechas para seleccionar **Auto**, pulsa <kbd>Enter</kbd> y vuelve a pulsar <kbd>Enter</kbd> para confirmar.
2. Escribe `/help` para ver los comandos de tu versión y pulsa <kbd>Esc</kbd> para cerrar la ayuda.
3. Haz una pregunta sencilla para comprobar que todo funciona:

   ```plaintext
   What are the key files in this project?
   ```

4. Lee la respuesta y observa cómo Copilot explora el repositorio antes de contestar.
5. Escribe `/mcp list` y confirma que el servidor MCP integrado de GitHub está disponible.
6. Pide a Copilot que localice la incidencia de filtrado:

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. Abre la URL y lee la incidencia. La usarás después de realizar un primer cambio pequeño.

> [!TIP]
> Una sesión normal de Copilot CLI trabaja en la rama seleccionada en el terminal; no crea automáticamente un árbol de trabajo. Crearás una rama de funcionalidad antes de cada cambio.

## Usar el atajo del taller

Normalmente, Copilot CLI pide permiso antes de usar herramientas fuera de los permisos establecidos. Para este taller volverás a iniciarlo con `--yolo`, un atajo aprobado por el usuario que elimina esas solicitudes de permiso dentro del codespace.

> [!CAUTION]
> `--yolo` habilita permisos automáticos completos (`--allow-all-tools`, `--allow-all-paths` y `--allow-all-urls`). Úsalo solo en un entorno aislado, como un codespace o una máquina virtual, y nunca como alias predeterminado para el desarrollo diario. Consulta [Permitir y denegar el uso de herramientas][allow-all-warning].

En el taller, `--enable-all-github-mcp-tools` habilita las herramientas MCP de lectura y escritura de GitHub para gestionar incidencias y solicitudes de incorporación de cambios. El codespace limita el acceso al equipo local, pero los recursos autenticados de GitHub siguen siendo reales. Revisa los cambios antes de publicarlos o combinarlos.

1. Sal de Copilot CLI con `/exit`.
2. Vuelve a iniciarlo desde la raíz del repositorio:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Haz otra pregunta sobre el proyecto para comprobar que la conversación funciona y, después, sal con `/exit`.

Copilot guarda las conversaciones automáticamente. Cuando cambies las instrucciones o añadas un agente, usarás `copilot --resume` para volver a la misma conversación y rama de la funcionalidad.

## Resumen y siguientes pasos

¡Enhorabuena! En esta lección:

- has instalado GitHub Copilot CLI con npm;
- te has autenticado con tu cuenta de GitHub;
- has confiado en el repositorio del taller y probado una conversación;
- has encontrado la incidencia de filtrado mediante el servidor MCP integrado de GitHub.

A continuación, [iniciarás tu primer cambio específico][next-lesson] y usarás Copilot CLI para mostrar las valoraciones por estrellas en las tarjetas de juego.

## Recursos

- [Instalar GitHub Copilot CLI][install-cli]
- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
