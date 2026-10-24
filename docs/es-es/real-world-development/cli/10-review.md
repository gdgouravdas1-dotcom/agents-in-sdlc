---
title: "Lección 10: repaso y próximos pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI en un flujo de trabajo continuo con Tailspin Toys. En concreto:

- has preparado un codespace, instalado Copilot CLI, explorado el proyecto y localizado la incidencia de filtrado del backlog;
- has añadido valoraciones por estrellas, revisado el resultado en un navegador reenviado y combinado manualmente tu primera solicitud de incorporación de cambios (PR);
- has partido de la incidencia de filtrado, definido el enfoque en Plan, implementado la funcionalidad en Autopilot y revisado el resultado en Interactive;
- has guiado al agente con instrucciones personalizadas, personalizado la habilidad `quality-checks` y ejecutado pruebas unitarias, lint y comprobaciones de tipos;
- has añadido el servidor Model Context Protocol (MCP) de Playwright y lo has usado para explorar el filtrado en un navegador real;
- has creado y seleccionado un agente personalizado de control de calidad (QA) para comprobar los requisitos, la cobertura, las comprobaciones y las pruebas del navegador;
- has revisado todos los cambios del filtrado y autorizado Agent Merge para combinar la PR;
- has explorado comandos de barra para gestionar el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

## Qué has entregado

El taller tiene dos hitos de PR, cada uno en su propia rama creada a partir de `main` actualizada:

1. **Valoraciones por estrellas:** muestra el valor `starRating` existente y un estado explícito para juegos sin valoración en las tarjetas.
2. **Filtrado y flujo de calidad:** implementa el filtrado, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la creación de su PR has usado la misma conversación y rama. El taller agrupa ese trabajo en una sola PR para simplificarlo.

## Distintas formas de verificar el código

Has comprobado el código con pruebas automatizadas, con una inspección manual en el navegador y con la exploración del navegador por parte de Copilot mediante MCP. La habilidad `quality-checks` ha ejecutado pruebas unitarias, lint y comprobaciones de tipos, y ha presentado los resultados en el formato nuevo. El agente de QA ha combinado esos resultados con la revisión de requisitos y cobertura antes de crear la PR.

Las pruebas añadidas deben resolver lagunas reales. Si la revisión de QA determina que no hace falta añadir pruebas, ese resultado también puede ser correcto. Antes de autorizar la combinación, revisa el código y las pruebas; si hay cambios, actualiza las pruebas de verificación afectadas.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller has actualizado instrucciones, personalizado una habilidad, creado un perfil de QA y configurado un servidor MCP. Reutiliza estas personalizaciones en otras conversaciones y adáptalas a las necesidades del equipo. Las instrucciones establecen estándares; las habilidades describen tareas repetibles; los agentes personalizados definen roles especializados; y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas, no solo el resumen del agente.

Elige el **modo y el modelo** según la tarea. Usa **Plan** para acordar un enfoque antes de implementarlo, **Interactive** para mantener el control en cambios acotados y **Autopilot** para tareas bien definidas. Elige un modelo rápido para ediciones rutinarias y uno más capaz para el trabajo complejo.

El contexto sigue siendo tan importante como la infraestructura. Explicar con claridad *qué* quieres crear, *por qué* y *cómo* influye significativamente en el resultado.

## Más ideas para explorar

Ya has completado el flujo principal. También puedes probar estas funciones de CLI:

- `/review` para pedir al agente de revisión de código que analice los cambios;
- `/rubber-duck` para pensar en voz alta y obtener otra perspectiva;
- `/fleet` para coordinar tareas independientes en paralelo;
- `/worktree` para aislar una tarea en un árbol de trabajo;
- `/delegate` para delegar una tarea al agente en la nube de Copilot.

## Próximos pasos

La mejor forma de mejorar con una herramienta es seguir utilizándola: para código de producción, proyectos personales o esa aplicación que llevas años queriendo crear. Comparte lo aprendido con tu equipo y aprende de sus experiencias. Y, como siempre, consulta la documentación.

Para ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Backer Concierge con Foundry][foundry]. Para comparar otros entornos, explora los talleres de [VS Code][vscode], la [aplicación GitHub Copilot][app] o el [agente en la nube de Copilot][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
