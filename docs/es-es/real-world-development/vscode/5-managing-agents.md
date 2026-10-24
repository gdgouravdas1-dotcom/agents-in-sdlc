---
title: "Ejercicio 5: supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Cuando usas el modo agente de GitHub Copilot, trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como el trabajo ocurre en tu equipo y en tiempo real, Copilot Chat en VS Code muestra las llamadas a herramientas y las ediciones que realiza. Puedes revisar las diferencias, aceptar o rechazar cambios y orientar la conversación con nuevas indicaciones sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad;
- revisarás y aceptarás las ediciones propuestas;
- guiarás al agente durante la sesión para añadir un control del modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad del [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita también un modo claro para usuarios a quienes les resulte demasiado oscuro el tema predeterminado. En lugar de empezar otra conversación, ampliarás la sesión actual.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en el chat con su diferencia y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada operación. Aparecerán lecturas y ediciones de archivos y, posiblemente, comandos de terminal.
3. Cuando el agente proponga una edición:
   - selecciona **Keep** para aceptar el cambio;
   - selecciona **Undo** para revertirlo y permitir que el agente lo intente de nuevo.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una operación requiere permiso, por ejemplo, ejecutar un comando de terminal, el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Mostrará un mensaje final con un resumen de los cambios.

## Guiar al agente para añadir el modo claro

Ahora que se ha implementado el modo de alto contraste, ampliarás la misma conversación para añadir un control del modo claro. Como sigues en la misma sesión, el agente conserva el contexto de los componentes modificados, los estilos y el patrón de almacenamiento local.

1. Envía esta indicación de seguimiento en Copilot Chat:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente revisará sus cambios anteriores y propondrá lo necesario para el modo claro.
3. Revisa cada edición nueva y selecciona **Keep** o **Undo**.
4. Si el agente toma una dirección que no quieres, por ejemplo, refactorizar los controles de una forma que no te gusta, detenlo con el botón **Stop** junto al cuadro de entrada y envía una indicación aclaratoria.

> [!TIP]
> Guiar una sesión en curso es más rápido que empezar de nuevo. Usa indicaciones de seguimiento para pedir ajustes («simplifica el CSS y usa variables CSS en vez de duplicar selectores»), corregir el rumbo («integra el modo claro en el panel de configuración actual; no añadas un tercer control») o ampliar el alcance («añade también atajos de teclado para ambos controles»).

## Revisar los cambios

Antes de confirmar el trabajo, revisa rápidamente todos los cambios del agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Debería incluir componentes Astro, estilos y pruebas relacionadas.
3. Abre varios archivos y examina las diferencias. Comprueba que se apliquen las pautas del agente de accesibilidad: atributos ARIA, navegación con teclado, HTML semántico y persistencia en el almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has usado el modo agente de Copilot Chat para supervisar y guiar una sesión en curso: has revisado cada edición, aceptado o rechazado cambios y enviado indicaciones de seguimiento sin perder el contexto.

En esta lección has aprendido a:

- supervisar en tiempo real el trabajo de un agente;
- aceptar y rechazar cambios de archivos;
- guiar una sesión en curso mediante indicaciones de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad como una solicitud de incorporación de cambios y revisarás los resultados del agente personalizado local.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo agente][agent-mode]

---

| [← Lección anterior: agentes personalizados][previous-lesson] | [Siguiente lección: iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
