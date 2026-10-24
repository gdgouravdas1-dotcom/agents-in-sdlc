---
title: "Lección 3: modos de agente, Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ya has añadido una pequeña funcionalidad al proyecto. Los cambios más amplios requieren un proceso más sólido. GitHub Copilot CLI está diseñado para integrarse con el flujo de trabajo de una organización y ayudarte a crear lo que se necesita de la forma adecuada. En esta lección empezarás un flujo de desarrollo con agentes: partirás de una incidencia, crearás una funcionalidad, validarás su código y comportamiento y, más adelante, la combinarás en el proyecto.

> [!NOTE]
> Usarás la misma conversación y rama durante todo el flujo de trabajo. Normalmente separarías las tareas en ramas o PR distintas, pero en este taller las agrupamos para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación de Copilot desde una incidencia de GitHub;
- definirás los requisitos en el modo Plan;
- implementarás la funcionalidad en el modo Autopilot;
- revisarás el código;
- validarás la funcionalidad manualmente en un navegador reenviado.

Más adelante actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks`, añadirás validación MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys crece y quienes lo visitan necesitan filtrar los juegos por categoría y editor. La incidencia del backlog describe esta funcionalidad, pero antes de programar hay que acordar detalles como la combinación de categorías. Usarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

La IA generativa no cambia los fundamentos del desarrollo de software; de hecho, los hace más importantes. El flujo habitual consiste en:

1. Partir de una incidencia que describa el trabajo.
2. Planificar lo que se debe crear.
3. Crear y revisar el código.
4. Ejecutar pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Combinar el código cuando se haya revisado y superado la integración continua.

> [!NOTE]
> Los detalles varían según el equipo y la organización, pero la mayoría sigue alguna variante de este proceso.

Seguir este enfoque ayuda a comprobar que el código generado por IA cumple los requisitos y pasa por el mismo proceso de validación que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** determina la autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: colaboras con el agente. Propone cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza con Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Empezar por la incidencia

Antes de empezar el filtrado, prepara el repositorio y el terminal de tu codespace.

1. Vuelve al codespace. Si está detenido, reinícialo.
2. Confirma que has combinado la PR de valoración por estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama:

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
8. Usa las flechas para seleccionar **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia a la indicación y volver a la pestaña **Session**.

La indicación empezará con `#7` o un número similar. El prefijo `#` permite incorporar al contexto una incidencia o PR de GitHub.

## Planificar el filtrado

La planificación permite definir el enfoque antes de delegar la implementación. Es especialmente útil en tareas complejas.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para activar Plan y comprueba que el indicador situado debajo de la indicación muestre **Plan**.
2. Después de la referencia a la incidencia, escribe:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot explorará el proyecto y decidirá cuál es el enfoque más adecuado.

3. Responde a las preguntas que plantee sobre el comportamiento de los filtros; elige las opciones que prefieras.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para ampliarlo.
5. Lee el plan y pide a Copilot que revise cualquier punto que no coincida con tus decisiones.

## Aprobar Autopilot

Después de revisar el plan, puedes pedir a Copilot que lo implemente. Autopilot le permite iterar hasta que considere que la tarea está completa.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de tu versión.
2. Comprueba que el indicador muestre **Autopilot**.
3. Observa cómo Copilot sigue el plan, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación de inmediato, así que revisa el plan antes. Si Copilot informa de dependencias ausentes o de un conflicto de puerto, resuelve primero ese problema de configuración.

## Revisar y verificar la implementación

Revisa el código generado antes de combinarlo, como harías con cualquier otro cambio.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para volver a Interactive. Confirma que el indicador ya no muestre **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y con las decisiones de planificación.
4. Cuando termines, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que corrija los fallos.

## Explorar la nueva funcionalidad

Ahora comprueba que el código se ejecuta. Inicia la aplicación y abre el sitio a través del puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba el filtrado por categoría, por editor y las combinaciones acordadas.
4. Comprueba que el reinicio y los resultados vacíos se comporten según la incidencia y tus decisiones.
5. Vuelve al codespace y pide a Copilot que detenga el servidor que inició.

## Resumen y siguientes pasos

Has usado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección:

- has iniciado una conversación de Copilot desde una incidencia de GitHub;
- has definido los requisitos en el modo Plan;
- has implementado la funcionalidad con Autopilot;
- has revisado el código;
- has validado la funcionalidad en un navegador reenviado.

A continuación, [explorarás las instrucciones personalizadas][next-lesson] y cómo ayudan a que el código siga las prácticas documentadas.

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
