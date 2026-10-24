---
title: "Ejercicio 2: agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: instrucciones personalizadas][previous-lesson] |
|:--|

Pocas organizaciones están libres de deuda técnica: incidencias de seguridad sin resolver, código heredado que requiere actualizaciones o funcionalidades que llevan tiempo en el backlog. El agente en la nube de GitHub Copilot puede actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, genera un borrador de solicitud de incorporación de cambios (PR) para que lo revise una persona. Así puedes delegar tareas tediosas, acelerar el desarrollo y centrarte en asuntos de mayor alcance.

En este ejercicio explorarás cómo:

- personalizar el entorno de generación de código;
- asegurarte de que las operaciones se realizan de forma segura;
- definir incidencias con un alcance claro;
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys quiere abordar cierta deuda técnica. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: de hecho, no hay ninguna. Como primer paso, el equipo quiere añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la forma de descubrir juegos. Quiere que la página de detalles de cada juego muestre títulos relacionados de la misma categoría para que los visitantes puedan seguir explorando. Por ahora no necesita una implementación pulida, sino algo que pueda usar para probar la experiencia de usuario. Aunque esta tarea es importante, hay otras incidencias con mayor prioridad.

Estas tareas pueden quedar relegadas fácilmente. El agente en la nube de Copilot puede trabajar en ellas de forma asíncrona mientras tú te ocupas de otras tareas; después podrás revisar su trabajo y comprobar que cumple los requisitos.

## Presentación del agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede trabajar en segundo plano de forma parecida a un desarrollador. Por ejemplo, puedes [asignar una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Tras la asignación, Copilot crea un borrador de PR para registrar el progreso, prepara un entorno y empieza a trabajar. Puedes examinar la sesión mientras está en curso o cuando haya terminado. Cuando la solución esté lista para revisarla, Copilot te mencionará en la PR.

## La importancia de definir bien el alcance

GitHub Copilot no hace magia: no elimina la necesidad de pensar. Incluso una tarea aparentemente sencilla puede tener cierta complejidad. Ten en cuenta las [recomendaciones para definir el alcance de las tareas del agente en la nube][cloud-agent-best-practices]. Trátalo como a un compañero de programación basado en IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian al incorporar IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas orientan a Copilot. Si ya has completado el ejercicio práctico de instrucciones, habrás visto `.github/copilot-instructions.md` en acción. Si no, ahora puedes echarle un vistazo. Antes de asignar trabajo al agente en la nube, revisa los archivos de instrucciones del repositorio para poder observar su efecto más adelante.

Abre los siguientes archivos en la interfaz web de GitHub o en un codespace:

- `.github/copilot-instructions.md`: revisa **Code standards**, especialmente las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el campo `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente la incidencia `Code lacks documentation`, comprueba si la PR resultante sigue las convenciones de TypeScript, incluye comentarios TSDoc y encabezados de archivo. Estas pautas proceden de los archivos de instrucciones y volverán a aparecer al revisar las PR en una lección posterior.

## Preparar el entorno de desarrollo del agente en la nube

La generación de código suele requerir un entorno concreto y scripts de configuración. Lo mismo ocurre cuando asignas tareas a Copilot: trabaja de forma parecida a un ingeniero de software.

El agente en la nube usa [GitHub Actions][github-actions] para preparar su entorno. Puedes personalizarlo con un [flujo de trabajo de configuración][setup-workflow] especial en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece a trabajar. Este flujo permite instalar herramientas y dependencias. El proyecto del laboratorio ya está configurado para proporcionar Node.js, las dependencias del proyecto, los binarios de navegador para las pruebas de extremo a extremo y la base de datos SQLite migrada y con datos para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Usa los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si los pasos de configuración clonan el repositorio, por ejemplo para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas, Copilot lo hará automáticamente cuando terminen los pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y la aplicación: Node.js (toda la aplicación usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y añade datos a la base de datos SQLite local para que la compilación y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene dos aspectos importantes:

- Incluye un único trabajo llamado `copilot-setup-steps`. Se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la PR.
- El desencadenador `workflow_dispatch` permite ejecutar el flujo manualmente desde la pestaña **Actions** del repositorio. Así puedes comprobar que funciona sin esperar a que Copilot lo ejecute.

## Añadir documentación

Muchos proyectos tienen información desactualizada o carecen de documentación. Esta deuda técnica ralentiza el trabajo y dificulta el mantenimiento y la incorporación de nuevos desarrolladores. Copilot puede ayudar a crear documentación, por lo que esta es una buena tarea para asignarle. Trabajará en segundo plano y revisarás el resultado en un ejercicio posterior.

1. Abre el repositorio en github.com en una pestaña nueva.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue**.
4. Selecciona **Blank issue**.
5. En **Title**, escribe `Code lacks documentation`.
6. En **Description**, escribe:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create**.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la PR que acaba de crear Copilot. Su título será similar a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un momento y actualiza el navegador.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer. Copilot está preparando el entorno mediante el flujo de trabajo descrito antes, analizando el proyecto y decidiendo cómo abordar la tarea.

13. Revisa la lista y las tareas que Copilot va a completar.
14. Desplázate por la cronología de la PR para ver cuándo Copilot empieza a trabajar en la incidencia.
15. Selecciona **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

16. Observa la sesión en directo y cómo Copilot explora el código, comprende el estado del proyecto, planifica y crea código.

La tarea puede tardar varios minutos. El agente en la nube permite realizar trabajo de forma asíncrona, así que aprovecharás ese tiempo asignándole otra tarea y después trabajarás en una funcionalidad de la aplicación.

## Añadir una sección de juegos relacionados a la página de detalles

Una ventaja del agente en la nube es que permite repartir el trabajo: tú puedes ocuparte de una tarea mientras el agente se ocupa de otra. Asignémosle la solicitud del equipo de diseño.

1. Vuelve al repositorio en github.com.
2. Selecciona **Issues** y, después, **New issue**.
3. Selecciona **Blank issue**.
4. En **Title**, escribe `Show related games on the game details page`.
5. En **Description**, escribe:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

6. Selecciona **Create**.
7. A la derecha, selecciona **Assign to Copilot**.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

8. Selecciona **Assign**.

Poco después, verás varios emojis 👀 en el primer comentario de la incidencia, que indican que Copilot está trabajando en ella. Copilot se encargará de esta segunda solicitud. No tienes que supervisar continuamente su trabajo; podrás revisar el resultado cuando termine. Ahora puedes centrarte en crear y usar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona en deuda técnica, nuevas funcionalidades o migraciones entre frameworks.

Has aprendido a:

- personalizar el entorno de generación de código;
- asegurarte de que las operaciones se realizan de forma segura;
- definir incidencias con un alcance claro;
- asignar incidencias a Copilot.

Mientras el agente trabaja, puedes pasar a [crear y usar agentes personalizados][next-lesson]. El agente en la nube también puede usar [servidores MCP][cloud-agent-mcp] y las instrucciones personalizadas que exploraste en los módulos anteriores.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
