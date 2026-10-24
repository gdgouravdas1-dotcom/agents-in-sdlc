---
title: "Lección 8: crear y combinar la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la habilidad, el perfil de QA y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, el cambio de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Ahora revisarás todo junto y abrirás una solicitud de incorporación de cambios (PR). Combinaste manualmente la PR de las valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> En un proyecto real dividirías la funcionalidad, las instrucciones, la habilidad y el agente de QA en varias PR. Para simplificar el taller, has mantenido todo el flujo de filtrado y calidad en una conversación y una rama, y lo incluirás en una sola PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de combinación;
- revisarás todos los cambios de la funcionalidad y las pruebas de verificación;
- crearás la PR del filtrado;
- activarás Agent Merge solo después de revisar los cambios y confirmarás que se ha combinado la PR.

## Escenario

Durante el flujo del filtrado, has usado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener bajo el control del desarrollador la autorización para combinarla.

## Presentación de Agent Merge

**Agent Merge** automatiza las tareas restantes para integrar una solicitud de incorporación de cambios. Al activarlo, Copilot resuelve los bloqueos de la PR, como las comprobaciones de integración continua (CI) fallidas, los comentarios de revisión o la necesidad de rebasar la rama. Después, activa la combinación automática de GitHub si el repositorio lo permite.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede ocuparse de esa responsabilidad. Revisa el trabajo y confirma que está listo antes de activarlo.

## Usar Agent Merge para gestionar la PR

Con todo el código creado, revisa los cambios, crea la PR y deja que Agent Merge gestione el resto del proceso.

1. Vuelve al codespace.
2. Abre el cuadro del agente con `/agent`.
3. Selecciona **Default** y pulsa <kbd>Enter</kbd>.
4. Crea una PR con `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará la integración continua de la PR y la combinará cuando todo termine correctamente.
7. Comprueba que Copilot muestre un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite aprobaciones obligatorias, protecciones de rama, colas de combinación, configuración del repositorio ni permisos ausentes. Si hay un bloqueo, lee el motivo y completa manualmente la combinación revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: generación, pruebas y validación de código, y ahora también el proceso de la PR. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de combinación;
- has revisado todos los cambios y las pruebas de verificación;
- has creado la PR del filtrado;
- has activado Agent Merge después de revisar el trabajo y confirmado que la PR se ha combinado.

A continuación, [explorarás otros comandos de barra de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
