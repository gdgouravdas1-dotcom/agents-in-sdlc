---
title: "Ejercicio 3: agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot son asistentes de IA especializados en tareas o ámbitos del flujo de desarrollo. Al definirlos con archivos Markdown en `.github/agents`, puedes proporcionar instrucciones, prácticas recomendadas, patrones de código y conocimientos específicos que ayuden a Copilot a realizar mejor ciertos tipos de trabajo. Los equipos pueden convertir su experiencia en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique las normas [WCAG][wcag], uno de seguridad o uno de pruebas.

Los agentes personalizados se definen mediante archivos Markdown en `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo incluye metadatos YAML con, al menos, `name` y `description`, seguidos de una indicación Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Comparación entre agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen algunas similitudes: ambos se definen principalmente con archivos Markdown y explican a la IA cómo realizar tareas. La diferencia más sencilla es que el **agente personalizado** es quien trabaja y las **habilidades** son las herramientas que puede utilizar.

Los agentes personalizados tienen su propia ventana de contexto y pueden coordinar habilidades e incluso otros agentes. En este laboratorio, el agente de accesibilidad revisará y actualizará el sitio conforme a directrices de accesibilidad; podría recurrir, por ejemplo, a una habilidad para gestionar solicitudes de incorporación de cambios o ejecutar pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como con cualquier herramienta de IA, prueba e itera para encontrar lo que funciona en tu entorno y para tus escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Con los agentes personalizados explorarás:

- cómo se definen;
- cómo asignarles una tarea.

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todo el mundo, independientemente de sus capacidades o preferencias visuales. Algunos usuarios han indicado que el tema oscuro actual es difícil de leer porque el contraste entre el texto y el fondo es insuficiente. El equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

Como la accesibilidad es prioritaria, usarás un agente personalizado para generar esta funcionalidad cuanto antes.

## Revisar el agente personalizado de accesibilidad

Ya hay un agente personalizado para accesibilidad. Revisa su contenido para entender cómo guiará a Copilot.

1. Vuelve al codespace y abre `.github/agents/accessibility.md`.
2. Fíjate en los campos `name` y `description` de los metadatos YAML.

> [!CAUTION]
> Los campos `name` y `description` son obligatorios en los metadatos de un agente personalizado.

3. Revisa las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio accesible;
   - las prácticas recomendadas de accesibilidad;
   - ejemplos de HTML, CSS y JavaScript;
   - errores y problemas habituales.

## Crear y asignar una incidencia

Mission Control es el centro para trabajar con los agentes del entorno. Desde allí puedes asignar tareas al agente en la nube, supervisarlas y redirigir una sesión para darle indicaciones adicionales. Asígnale la tarea de crear el modo de alto contraste.

1. Abre tu repositorio.
2. Selecciona **Issues** y, después, **New issue**.
3. Selecciona **Blank issue**.
4. En **Title**, escribe `Add high contrast mode to website`.
5. En **Description**, escribe:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

6. Selecciona **Create**.
7. A la derecha, selecciona **Assign to Copilot**.
8. Selecciona **Accessibility agent** en la lista de agentes personalizados.

    ![Asignación del agente en la nube con el agente de accesibilidad resaltado](../../../_images/ex5-select-custom-agent.png)

9. Selecciona **Assign**. Copilot empezará a trabajar en segundo plano.

## Resumen y siguientes pasos

En esta lección has explorado los [agentes personalizados][custom-agents], asistentes especializados en tareas y ámbitos concretos. Te permiten convertir la experiencia y los estándares de tu equipo en agentes reutilizables que ayudan a Copilot a realizar determinados tipos de trabajo.

Has aprendido a:

- definir agentes personalizados;
- asignarles una tarea.

Mientras Copilot implementa el modo de alto contraste, puedes [supervisar y guiar la sesión del agente][next-lesson] desde Mission Control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Preparar la organización para usar agentes personalizados][org-custom-agents]
- [Preparar la empresa para usar agentes personalizados][enterprise-custom-agents]

---

| [← Lección anterior: agente en la nube de GitHub Copilot][previous-lesson] | [Siguiente lección: supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
