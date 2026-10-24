---
title: "Lección 9: comandos de barra de GitHub Copilot CLI"
description: "Explora los comandos de barra para gestionar el contexto, elegir modelos, compartir sesiones y delegar tareas opcionalmente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como otras buenas herramientas de CLI, GitHub Copilot CLI incluye comandos de barra para interactuar con la aplicación. Estos comandos ofrecen funciones avanzadas, información interna y opciones de configuración adicionales. Ya has usado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Ahora explorarás otros útiles.

En esta lección:

- usarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación;
- usarás `/model` para ver los modelos disponibles;
- aprenderás cómo exportar o compartir una sesión con `/share`;
- explorarás comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas al agente en la nube.

## Escenario

Has terminado el flujo principal de CLI. Ahora revisarás algunas funciones adicionales: gestión del contexto, cambio de modelo, uso compartido de sesiones y delegación opcional al [agente en la nube de Copilot][about-cloud-agent].

## Explorar el contexto de Copilot CLI

En tareas extensas o complejas, puede que te acerques al límite de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando lo necesita; también puedes consultar o compactar el contexto manualmente.

1. Vuelve al codespace e inicia Copilot CLI desde la raíz del repositorio, si aún no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Revisa el modelo, el uso actual de tokens y la distribución del contexto entre instrucciones del sistema, herramientas, mensajes y espacio libre.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara el resultado. Si la conversación ya era breve, puede que apenas haya cambiado.

> [!NOTE]
> Copilot CLI compacta automáticamente el contexto cuando se llena la ventana. Usa `/compact` para elegir cuándo hacerlo. Si empiezas una tarea distinta y quieres una conversación nueva, usa `/clear` o `/new`.

## Elegir un modelo

Los distintos modelos tienen fortalezas diferentes y cada desarrollador tiene sus preferencias. Copilot CLI permite consultar y seleccionar el modelo.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información de uso.
3. Mantén el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Compartir una sesión

Compartir el trabajo y lo aprendido ayuda a todo el equipo. El comando `/share` puede exportar una sesión a Markdown o HTML, crear un enlace para compartir o publicar un gist de GitHub.

1. Escribe `/help` y revisa las opciones de `/share` disponibles en tu versión.
2. Si quieres compartir la sesión, selecciona el destino adecuado, por ejemplo `/share file` para exportarla a un archivo Markdown local.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones pueden incluir indicaciones, respuestas y detalles del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio o de la conversación que el equipo no quiera compartir.

## Opcional: ampliar el trabajo o delegarlo

El taller principal ha terminado. Copilot CLI también incluye comandos para tareas más amplias:

- `/fleet` puede dividir tareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo de Git aislado para otra tarea.
- `/delegate` puede enviar una tarea al agente en la nube de Copilot, que trabaja de forma asíncrona y puede abrir una solicitud de incorporación de cambios.

Estos comandos son opcionales porque pueden crear árboles de trabajo adicionales o iniciar trabajo remoto. Antes de probarlos, define una tarea nueva y bien acotada, y revisa los resultados con el flujo habitual. Para profundizar en el trabajo asíncrono con agentes, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y siguientes pasos

Los comandos de barra permiten configurar Copilot CLI, compartir sesiones y consultar su funcionamiento interno. En esta lección:

- has usado `/context` y `/compact` para explorar la gestión del contexto;
- has usado `/model` para consultar modelos;
- has aprendido cómo exportar o compartir sesiones con `/share`;
- has explorado los comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas.

Copilot CLI incluye más comandos de barra. Para terminar el recorrido, [repasarás lo aprendido y los próximos pasos][next-lesson].

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca del agente en la nube de Copilot][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
