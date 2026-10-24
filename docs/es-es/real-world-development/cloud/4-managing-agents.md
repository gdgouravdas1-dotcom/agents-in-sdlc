---
title: "Ejercicio 4: supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los ejercicios anteriores has pedido al agente en la nube de Copilot que se ocupe de tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente trabaja de forma autónoma y asíncrona, sigue siendo importante supervisar esas tareas.

Puedes gestionar las tareas asignadas al agente desde distintas herramientas, como la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas con solicitudes de incorporación de cambios abiertas, examinar las acciones realizadas y orientar una sesión que siga en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas;
- guiarás una sesión en curso para solicitar funcionalidad adicional.

## Escenario

Después de asignar al agente la creación de un modo de alto contraste, el equipo se da cuenta de que también sería buen momento para añadir un modo claro. Como ya se están actualizando los estilos del sitio y añadiendo controles de modo, pedirás al agente que implemente ambos modos.

## Revisar las tareas del agente en la nube de Copilot

Consulta el estado de las tareas asignadas al agente:

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Examina las tareas del panel principal y del panel izquierdo. Deberías encontrar tareas como:
   - actualizar la documentación del código;
   - generar API para modificar juegos;
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una tarea en curso. Revisa las operaciones que Copilot ha realizado, que pueden incluir:
   - obtener el código del repositorio;
   - preparar el entorno de trabajo;
   - configurar servidores MCP;
   - completar diferentes pasos de la tarea.

> [!NOTE]
> Los pasos exactos dependerán del estado del trabajo y del enfoque que haya elegido Copilot.

4. Observa también el panel de solicitudes de incorporación de cambios que aparece a la derecha. Allí puedes consultar la PR y los archivos modificados.

## Guiar al agente en la nube

Ahora pedirás a Copilot que añada un control para el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión que corresponde al modo de alto contraste. El título exacto puede variar según el que haya elegido Copilot y el estado del trabajo.

    ![Sesión de accesibilidad en Mission Control](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos hasta que la sesión indique que ha terminado de configurarse y ha empezado a trabajar. Lo sabrás cuando aparezcan mensajes de actividad.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarea del agente en el cuadro de diálogo para guiar una sesión activa](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el mensaje.
5. Observa cómo Copilot confirma la solicitud y la incorpora a su trabajo.

## Dejar que Copilot trabaje

Copilot incorporará la nueva solicitud después de completar el paso que estaba realizando cuando enviaste el mensaje. La tarea tardará varios minutos, así que puedes aprovechar el tiempo para repasar lo que has aprendido.

## Resumen y siguientes pasos

Has explorado la página de agentes de Copilot, el centro de control para supervisar y guiar las tareas del agente en la nube. Allí puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir una sesión en curso para ajustar su alcance o aportar más indicaciones.

Has aprendido a:

- usar Mission Control y la página de agentes para supervisar las tareas;
- redirigir una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot completa el trabajo de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: agentes personalizados][previous-lesson] | [Siguiente lección: iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
