---
title: "Lección 7: crear y usar un agente de QA"
description: "Crea un agente personalizado de QA que combine los requisitos de la incidencia, la habilidad quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has usado la habilidad `quality-checks` para ejecutar comprobaciones automatizadas y Playwright MCP para observar el filtrado en el navegador. Ahora reunirás estas capacidades en un agente personalizado con un proceso de QA bien definido.

En esta lección:

- explorarás cómo un agente personalizado usa instrucciones, habilidades y herramientas MCP;
- crearás y revisarás un perfil de control de calidad reutilizable;
- seleccionarás el agente de QA y revisarás sus conclusiones sobre la incidencia de filtrado.

## Escenario

Antes de abrir una solicitud de incorporación de cambios (PR), Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador. Un agente personalizado puede coordinar el proceso de QA y generar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una variante especializada de Copilot definida en un perfil Markdown. El perfil explica su objetivo, sus instrucciones y las herramientas disponibles. En este taller crearás el rol de QA en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Las personalizaciones que has usado cumplen funciones distintas: las instrucciones del repositorio describen los estándares del equipo; la habilidad `quality-checks` agrupa comprobaciones repetibles; Playwright MCP proporciona herramientas para el navegador; y el perfil de QA indica a Copilot cómo combinar esas capacidades para comprobar los requisitos y comunicar los resultados. No sustituye a las demás personalizaciones ni requiere otra conversación.

## Crear el perfil de QA

Antes de abrir la PR, pide a Copilot que cree un perfil de QA reutilizable. El perfil definirá las comprobaciones y los límites que debe respetar.

1. Vuelve al codespace y confirma que la conversación del filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree el agente personalizado:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Revisar el perfil

Antes de usar el agente, confirma que Copilot haya incluido el flujo de QA y los límites previstos. Así evitarás que el agente modifique la funcionalidad cuando solo quieres verificarla.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee los metadatos. `description` es obligatorio; `name` es opcional, aunque proporciona un nombre visible claro.
3. Comprueba que el perfil empieza por los requisitos, sigue las instrucciones del repositorio, ejecuta la habilidad `quality-checks` y usa Playwright MCP.
4. Confirma que el informe incluya pruebas que respalden los resultados, que el agente pida permiso antes de modificar código de implementación y que no confirme cambios ni abra PR.
5. Si falta alguna responsabilidad o límite, pide al agente predeterminado que revise el perfil antes de continuar.

## Ejecutar QA para la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Después de crear el perfil, retoma la misma conversación del filtrado para que el agente de QA disponga de la incidencia y las decisiones de planificación.

1. Activa el agente con esta indicación:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, quizá no aparezca en la lista de agentes disponibles. Actívalo con el comando indicado.

2. Pide al agente que revise la funcionalidad:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. Espera a que el agente de QA complete la revisión.
4. Lee el informe.

## Resumen y siguientes pasos

Has añadido un rol especializado reutilizable al flujo de trabajo y has revisado sus resultados. En esta lección:

- has explorado cómo un agente personalizado usa instrucciones, habilidades y herramientas MCP;
- has creado y revisado un perfil de QA;
- has seleccionado el agente de QA y revisado sus conclusiones sobre la incidencia.

Ahora tienes la implementación, la actualización de la habilidad, el perfil de QA, las pruebas y el informe de verificación. A continuación, [reunirás el trabajo en una PR de la funcionalidad y usarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
