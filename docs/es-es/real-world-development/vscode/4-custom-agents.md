---
title: "Ejercicio 4: agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot son asistentes de IA especializados en tareas o ámbitos concretos del flujo de desarrollo. Al definirlos mediante archivos Markdown en `.github/agents`, puedes proporcionar instrucciones, prácticas recomendadas, patrones de código y conocimientos específicos para que Copilot realice mejor determinadas tareas. Los equipos pueden convertir su experiencia en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique las normas [WCAG][wcag], uno de seguridad o uno de pruebas.

Los agentes se definen mediante archivos Markdown en `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo incluye metadatos YAML con, al menos, `name` y `description`, y un prompt Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Comparación entre agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen algunas similitudes: ambos se definen principalmente con Markdown y explican a la IA cómo realizar tareas. La diferencia más sencilla es que el **agente personalizado** es quien trabaja y las **habilidades** son las herramientas que utiliza.

Los agentes tienen una ventana de contexto propia y pueden coordinar habilidades e incluso otros agentes. En este taller, el agente de accesibilidad revisará y actualizará el sitio conforme a las directrices de accesibilidad; durante ese trabajo podría usar habilidades para gestionar solicitudes de incorporación de cambios o ejecutar pruebas.

> [!NOTE]
> No existe una única forma correcta de crear agentes personalizados. Prueba e itera para encontrar lo que funciona mejor en tu entorno y para tus escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todas las personas, independientemente de sus capacidades o preferencias visuales. Algunos usuarios han indicado que el tema oscuro actual es difícil de leer debido al contraste insuficiente entre el texto y el fondo. El equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

La accesibilidad es importante, así que usarás un agente personalizado para generar esta funcionalidad cuanto antes.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente;
- usarás el agente en Copilot Chat para implementar un modo de alto contraste.

## Revisar el agente personalizado de accesibilidad

Ya hay un agente personalizado para accesibilidad. Revisa su contenido para entender cómo guiará a Copilot.

1. Vuelve al codespace y abre un terminal.
2. Crea una rama nueva desde `main` para el trabajo de accesibilidad, separado de la PR de filtrado del ejercicio 3:

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

3. Abre `.github/agents/accessibility.md`.
4. Revisa los campos `name` y `description` de los metadatos YAML.

> [!CAUTION]
> Los metadatos `name` y `description` son obligatorios en los agentes personalizados.

5. Examina las secciones siguientes, que describen:
   - responsabilidades básicas para generar código de sitios accesibles;
   - prácticas recomendadas de accesibilidad;
   - ejemplos de HTML, CSS y JavaScript;
   - errores y problemas habituales.

## Usar el agente personalizado en Copilot Chat

VS Code muestra los agentes personalizados definidos en `.github/agents` en el selector de agentes de Copilot Chat. Selecciona un agente para limitar la conversación a sus instrucciones y herramientas.

> [!TIP]
> **Abrir Copilot Chat**
>
> Vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían según el ejercicio; cada paso los indicará cuando sea necesario.

1. En el selector de la vista Chat, selecciona **Agent** si aún no está seleccionado.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona el menú de agentes situado en la parte inferior de la vista Chat, que muestra el agente activo (de forma predeterminada, **default**).
3. Selecciona **Accessibility agent**.
4. Envía al agente de accesibilidad esta indicación:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat explorará el código, propondrá cambios y los aplicará a los archivos. En el chat aparecerá cada edición con la ruta del archivo y una diferencia que podrás revisar.
6. El indicador **Files changed** se actualizará para mostrar el conjunto de archivos modificados.

> [!NOTE]
> El proceso puede tardar varios minutos. Copilot modificará archivos del proyecto, como componentes Astro, estilos y pruebas relacionadas.

Revisarás y guiarás el trabajo en curso en el siguiente ejercicio.

## Resumen y siguientes pasos

Has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes especializados en tareas y ámbitos concretos. Te permiten convertir la experiencia y los estándares del equipo en agentes reutilizables.

Has aprendido a:

- definir agentes personalizados;
- usar un agente personalizado en el modo agente de Copilot Chat.

A continuación, [supervisarás y guiarás el trabajo del agente][next-lesson], revisarás los cambios a medida que se produzcan y añadirás un control para el modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: probar la funcionalidad con Playwright MCP][previous-lesson] | [Siguiente lección: supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
