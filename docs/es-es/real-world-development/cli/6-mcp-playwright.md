---
title: "Lección 6: validar la funcionalidad con Playwright MCP"
description: "Configura Playwright MCP en Copilot CLI y úsalo para explorar la funcionalidad de filtrado en un navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Crear código implica algo más que escribirlo: también hay que trabajar con datos y servicios externos y ofrecer automatizaciones adicionales a Copilot. Para ello sirven los servidores MCP, que amplían las herramientas y los servicios disponibles en Copilot CLI.

En esta lección:

- aprenderás qué es Model Context Protocol (MCP) y cómo lo usa Copilot CLI;
- añadirás el servidor MCP de Playwright si aún no está disponible;
- pedirás al agente que controle un navegador y explore la funcionalidad de filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar cambios de interfaz hay que interactuar con ella. Permitirás que Copilot use el sitio como lo haría una persona, para automatizar mejor las comprobaciones y aumentar la confianza en el resultado.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real, accedan a información actualizada y realicen acciones en tu nombre.

Un servidor MCP actúa como puente entre el agente y las herramientas o servicios externos. Cada servidor ofrece un conjunto distinto de herramientas y recursos:

- **[Servidor MCP de GitHub][github-mcp]**: proporciona acceso a API para gestionar repositorios, incidencias y solicitudes de incorporación de cambios de GitHub.
- **[Servidor MCP de Playwright][playwright-mcp-server]**: ofrece automatización del navegador con Playwright.

Hay muchos otros servidores MCP. GitHub aloja un [registro MCP][mcp-registry] para facilitar su descubrimiento y las contribuciones.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, verifica quién lo publica y valora las implicaciones de seguridad.

## Añadir el servidor MCP de Playwright

Si el servidor MCP de Playwright todavía no está disponible, añádelo para que Copilot pueda interactuar con el sitio como lo haría una persona.

> [!NOTE]
> Si Copilot CLI ya tiene Playwright MCP configurado, omite estos pasos.

1. Vuelve al codespace.
2. En Copilot CLI, escribe:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Confirma **STDIO** como tipo de servidor con <kbd>Enter</kbd> y vuelve a pulsar <kbd>Tab</kbd>.
5. En el cuadro **Command**, pega:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro MCP.

## Pedir a Copilot que explore la funcionalidad con Playwright

Ya has comprobado manualmente la funcionalidad. Ahora pedirás al servidor MCP de Playwright que haga lo mismo desde Copilot.

1. Envía esta indicación:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Normalmente Copilot deducirá por sí mismo que debe usar el servidor MCP. Aun así, indicárselo puede orientar mejor el trabajo y hacer que los resultados sean más coherentes.

2. Observa cómo Copilot realiza las operaciones en el navegador.
3. Lee el informe y comprueba que todo funciona como se esperaba.

Copilot iniciará el servidor, interactuará con el sitio mediante Playwright, detendrá el servidor y te entregará un informe.

## Resumen y siguientes pasos

Has usado el servidor MCP de Playwright para explorar tu funcionalidad en un navegador real desde Copilot CLI. En esta lección:

- has aprendido qué es MCP y cómo lo usa Copilot CLI;
- has añadido Playwright MCP si aún no estaba disponible;
- has pedido al agente que controle un navegador y explore el filtrado.

A continuación, [crearás un agente personalizado de QA][next-lesson] que combine la habilidad y las herramientas del navegador en un rol especializado.

## Recursos

- [Qué es MCP y por qué todo el mundo habla de ello][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
