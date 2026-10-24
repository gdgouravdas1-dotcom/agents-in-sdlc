---
title: "Ejercicio 2: añadir funcionalidad con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas suelen requerir cambios en varios archivos y tareas como ejecutar pruebas. Normalmente, el flujo consiste en localizar todos los archivos necesarios, hacer los cambios, ejecutar pruebas, depurar y averiguar qué archivo falta modificar. El modo de agente de Copilot puede ayudar con este proceso.

El modo de agente funciona de forma más autónoma en el IDE: explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. En este ejercicio lo usarás para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque sus nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE y permite ciclos de respuesta rápidos. El agente en la nube trabaja de forma asíncrona como otro miembro del equipo y se comunica mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- usar el modo de agente para explorar el proyecto, localizar archivos pertinentes y coordinar cambios;
- implementar funcionalidades en la interfaz y la capa de datos;
- revisar los cambios y las pruebas generadas antes de combinar el código.

## Escenario

El catálogo de juegos está creciendo y quieres que los usuarios puedan filtrar por categoría y editor. Ya has creado un auxiliar para los editores en el ejercicio anterior. Ahora completarás el trabajo restante de la capa de datos, la interfaz y las pruebas con el modo de agente.

## Ejecutar el sitio web de Tailspin Toys

Antes de realizar cambios, explora el sitio para conocer su funcionamiento actual. Tailspin Toys es una plataforma de financiación colectiva para juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles.

El sitio es una sola aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM desde sus metadatos; no hay una API de backend ni un framework de interfaz en el cliente. Los auxiliares reutilizables de acceso a datos están en `src/lib/`, y la interactividad se implementa con pequeños bloques `<script>` de Astro y las API estándar del DOM.

### Iniciar el sitio web

El script `npm run dev` inicia el servidor de desarrollo Astro:

1. Vuelve al codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor Astro esté listo, verás un mensaje similar a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pregunta si quieres abrir `http://localhost:4321` en el navegador, ciérralo seleccionando **x**.

5. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar el enlace local desde el terminal se redirige a `https://<your-codespace-name>-4321.app.github.dev/`, un túnel privado al servidor del codespace.

### Explorar el sitio

Cuando se ejecute, explora sus funciones principales:

- **Página principal**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: muestra información adicional, como el título, la descripción, el editor y la categoría.

## Explorar el backlog con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de continuar, abre el panel Copilot Chat en el codespace y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían según el ejercicio; cada paso los indicará cuando sea necesario.

La primera versión del sitio funciona, pero queremos añadir funciones. El proyecto de plantilla ya creó un backlog de incidencias: pide a Copilot que lo muestre.

1. En el selector de agentes de la vista Chat, selecciona **Agent**. Este agente planifica e implementa cambios en varios archivos, ejecuta comandos de terminal e invoca herramientas.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores del taller no expresan preferencia por ningún modelo. Durante la creación del taller usamos Claude Sonnet 4.5 para que las sugerencias fueran relativamente coherentes. Como los LLM son probabilísticos, el resultado puede variar; es normal.

> [!NOTE]
> Debido a la naturaleza probabilística de los LLM, Copilot puede usar otro comando MCP y completar igualmente la tarea.

3. Envía a Copilot esta indicación:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera las incidencias.
5. Revisa la lista generada.

Copilot también habrá priorizado las incidencias según el valor que considera que aportan a los usuarios.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporciona contexto sobre el desarrollo de funcionalidades.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las pautas generales, entre ellas:
   - la arquitectura;
   - los principios de diseño de componentes, pruebas y accesibilidad;
   - los enlaces a instrucciones específicas para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar a otros archivos de instrucciones y a archivos del proyecto. Como las rutas son relativas al archivo de instrucciones, puedes reutilizar pautas, dividir instrucciones complejas y señalar ejemplos.

## Implementar el filtrado

Para completar el filtrado se necesitan al menos tres cambios:

- añadir o ajustar la lógica de filtrado en la capa de datos (`src/lib/`);
- añadir o actualizar pruebas del comportamiento de filtrado;
- actualizar la página de lista de juegos para incluir los controles de filtrado.

Las pruebas deben superarse antes de combinar los cambios. Pide al modo de agente que implemente la funcionalidad:

1. Continúa la conversación actual o selecciona **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Selección de las instrucciones de interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** siga seleccionado.
4. Comprueba que **Claude Sonnet 4.5** siga seleccionado.
5. Pide a Copilot que implemente la funcionalidad a partir de la incidencia relacionada:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos de datos, interfaz y pruebas. Después, modificará los archivos y ejecutará las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot explorará el proyecto, modificará archivos y ejecutará pruebas. El proceso puede tardar varios minutos. Mientras trabaja, el editor puede mostrar los controles **Keep** y **Undo**. Espera a que termine antes de decidir si conservas o deshaces cada cambio.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Solicitud para confirmar la ejecución de pruebas en Copilot Chat](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que haya pausas o que algunas pruebas fallen durante el proceso. Copilot alternará entre generar código y ejecutar pruebas hasta terminar sin detectar errores.

   ![Sesión de Copilot Agent Mode terminada](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examina el código generado en busca de problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! Has usado el modo de agente de GitHub Copilot para añadir capacidades al sitio de Tailspin Toys. Has aprendido a:

- implementar funciones en la interfaz y la capa de datos;
- explorar el proyecto, localizar archivos y coordinar cambios;
- revisar el código y las pruebas antes de combinarlo.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una PR.

### Ejercicio adicional: implementar la paginación

A medida que crezca la lista de juegos, será necesario paginarla. Usa las habilidades de este ejercicio y pide a Copilot que implemente la paginación. Para ello:

- sigue las prácticas recomendadas y usa los archivos de instrucciones existentes;
- decide si el usuario podrá elegir el tamaño de página o si será fijo;
- proporciona a Copilot las pautas necesarias en la indicación;
- itera con Copilot, pidiendo cambios y aportando contexto.

## Recursos

- [Modos Ask, Edit y Agent de Copilot: para qué sirven y cuándo usarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
