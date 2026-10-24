---
title: "Ejercicio 3: probar la funcionalidad con el servidor MCP de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: modo agente][previous-lesson] · [Siguiente lección: agentes personalizados →][next-lesson]

Acabas de crear la funcionalidad de filtrado con el modo agente. Antes de abrir una solicitud de incorporación de cambios (PR), comprueba que funciona en el navegador. En lugar de recorrer la aplicación tú mismo, conectarás el **servidor MCP de Playwright** y dejarás que Copilot controle un navegador real para probar la funcionalidad. Después, publicarás la rama y abrirás la PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían Copilot con nuevas herramientas;
- añadirás el servidor MCP de Playwright al espacio de trabajo;
- pedirás a Copilot que lo use para probar manualmente el filtrado en el navegador;
- publicarás la rama y abrirás una PR para el trabajo de filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo agente es mucho más potente cuando puede acceder a recursos fuera del editor. **Model Context Protocol (MCP)** es un estándar que permite a Copilot comunicarse con herramientas y servicios externos.

![Diagrama del funcionamiento del modo agente y su interacción con el contexto, el LLM y las herramientas, incluidas las aportadas por servidores MCP y extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos. Así pueden acceder a información actualizada y realizar acciones en tu nombre.

Un servidor MCP actúa como puente entre el agente y las herramientas o servicios externos, como API existentes o herramientas locales de paquetes npm. Cada servidor proporciona un conjunto de herramientas y recursos distinto.

Algunos servidores MCP populares:

- **[Servidor MCP de GitHub](https://github.com/github/github-mcp-server)**: proporciona acceso a las API para gestionar repositorios de GitHub, crear repositorios, actualizar los existentes y gestionar incidencias y solicitudes de incorporación de cambios.
- **[Servidor MCP de Playwright](https://github.com/microsoft/playwright-mcp)**: automatiza navegadores con Playwright; permite navegar por páginas, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP. GitHub aloja un [registro MCP](https://github.com/mcp) para facilitar su descubrimiento y las contribuciones.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, verifica quién lo publica y valora las implicaciones de seguridad. Usa solo servidores de confianza y ten cuidado al conceder acceso a operaciones o recursos confidenciales.

## Revisar la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo.

1. Abre `.vscode/mcp.json` en el codespace.
2. Verás que ya hay un servidor `github` configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

La plantilla del proyecto incluye esta entrada `github`, por eso Copilot pudo leer las incidencias del backlog en el ejercicio anterior. Usa el [servidor MCP remoto de GitHub][remote-github-mcp-server], así que no hay que instalar nada localmente. VS Code se conecta mediante HTTP y te autenticas con GitHub la primera vez que Copilot usa sus herramientas.

## Añadir el servidor MCP de Playwright

Añadirás un segundo servidor. El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador controlable, necesario para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github`:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` se ejecuta localmente: VS Code lo inicia con `npx @playwright/mcp@latest`. La opción `--headless` ejecuta el navegador sin ventana visible, algo necesario en un codespace, que no tiene escritorio.

> [!NOTE]
> Tailspin Toys ya usa Playwright para las pruebas de extremo a extremo, así que el navegador necesario normalmente ya está instalado. Si Copilot indica que falta, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

## Iniciar y autorizar el servidor

VS Code inicia los servidores MCP **bajo demanda**: cuando Copilot necesita sus herramientas por primera vez, VS Code inicia el servidor y te pide que confirmes que confías en él.

1. En `.vscode/mcp.json`, selecciona **Start**, que aparece encima de la entrada `playwright`, para comprobar que la configuración es correcta.
2. Si VS Code pide que confirmes que confías en el servidor, revisa la configuración y autorízalo.

Iniciarlo manualmente es opcional. Si omites este paso, VS Code lo iniciará y te pedirá autorización cuando Copilot necesite las herramientas.

## Probar la funcionalidad de filtrado

El servidor MCP de Playwright interactúa con el [árbol de accesibilidad][playwright-mcp-server] de la página, no con capturas de pantalla. Así, el agente razona sobre elementos estructurados y etiquetados, como botones, enlaces y elementos de lista, de forma similar a las tecnologías de asistencia. Una comprobación funcional rápida también ofrece una revisión básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que compruebe el filtrado:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot abrirá un navegador con el servidor MCP de Playwright, seguirá los pasos e informará de los resultados. Compara el informe con los criterios de aceptación de la incidencia. Si algo no funciona, haz preguntas de seguimiento o pide a Copilot que corrija el código antes de abrir una PR.

> [!NOTE]
> La aplicación debe estar disponible en `http://localhost:4321`. Si has detenido el servidor de desarrollo, vuelve a iniciarlo antes de enviar la indicación. La primera vez que Copilot use Playwright MCP puede que necesite descargar un navegador. Si falta, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar la rama y crear una solicitud de incorporación de cambios

Ahora que has comprobado que la funcionalidad funciona, publicarás la rama `filtering-vscode` y abrirás una PR para que la revise el equipo.

1. Abre el panel **Source Control** del codespace y revisa los cambios de Copilot.
2. Selecciona el icono **+** para preparar los cambios.
3. Selecciona el botón **Sparkle** para generar un mensaje de confirmación.

   ![Cambios de Copilot en el panel Source Control](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crear la solicitud de incorporación de cambios

Copilot puede buscar la incidencia pertinente y asociarla a la PR.

1. Abre Copilot Chat y selecciona **New Chat**.
2. En el selector de agentes, confirma que esté seleccionado **Agent** para que Copilot pueda usar las herramientas de GitHub.
3. Pide a Copilot que cree la PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Si hace falta, selecciona **Continue** para permitir que Copilot recopile información y realice operaciones. La primera vez que use una herramienta de GitHub puede que tengas que autenticarte.
5. Observa cómo busca la incidencia correcta y crea la PR.
6. Selecciona el enlace que genere Copilot y revisa la PR, pero **no la combines todavía**.

## Resumen y siguientes pasos

En esta lección:

- has aprendido qué es MCP y cómo los servidores MCP amplían Copilot con herramientas;
- has añadido el servidor MCP de Playwright al espacio de trabajo;
- lo has usado para probar el filtrado en un navegador antes de publicarlo;
- has publicado la rama y abierto una PR.

Has usado un servidor MCP para probar una funcionalidad. MCP también puede ampliar Copilot de otras formas. A continuación, [crearás un agente personalizado][next-lesson] para facilitar tareas especializadas, como las revisiones de accesibilidad.

## Recursos

- [Qué es MCP y por qué todo el mundo habla de ello][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
