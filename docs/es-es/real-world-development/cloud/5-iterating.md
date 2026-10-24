---
title: "Ejercicio 5: iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: gestión de agentes][previous-lesson] |
|:--|

## Revisar el trabajo

Durante este laboratorio has colaborado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le has pedido que documente el código, cree una sección de juegos relacionados y añada funciones de accesibilidad, como los modos de alto contraste y claro. Ahora revisarás los cambios de código y, si hace falta, pedirás a Copilot que los mejore.

### Escenario

Los fundamentos del diseño de software y DevOps no cambian al incorporar IA generativa. Siempre hay que revisar el código generado y seguir el proceso normal de DevOps. Antes de habilitar la revisión para el resto del equipo, revisarás las sugerencias de Copilot para la documentación, los juegos relacionados y la accesibilidad.

## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube trabaja de forma asíncrona y sin supervisión, se aplican varias medidas de seguridad:

- Copilot tiene acceso de lectura al repositorio y acceso de escritura **solo** a la rama que usa para su trabajo.
- El agente se ejecuta en GitHub Actions, en un entorno efímero y separado.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de que se ejecuten.
- De forma predeterminada, el acceso a recursos externos, incluidos los servidores MCP, está limitado. Consulta cómo [configurar el firewall del agente en la nube](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall).

## Revisar la documentación generada

Empieza por la primera solicitud de incorporación de cambios (PR) que creó el agente: la documentación del código. La revisarás con la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Puede que veas una advertencia de que el firewall ha bloqueado a Copilot. Es normal, porque el acceso a recursos externos, incluidas las llamadas a servidores MCP externos, está limitado de forma predeterminada. Si quieres, puedes [configurar o desactivar el firewall del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests**.
3. Abre la PR titulada, por ejemplo, **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR mostrará la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Cuando la PR esté lista, selecciona **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Examina el código actualizado y la documentación nueva. Los cambios exactos pueden variar. Comprueba si hay comentarios TSDoc, si se siguen las convenciones de TypeScript y si los archivos incluyen encabezados de comentarios. Estas directrices proceden de los archivos de instrucciones que revisaste al principio del ejercicio 2.
6. Cuando termines, vuelve a **Conversation** y desplázate hacia abajo.
7. Comprueba si hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Comprueba que los flujos aparecen en la sección de comprobaciones de la PR y que se superan las comprobaciones del proyecto para la aplicación Astro. Puede tardar unos minutos.

## Pedir cambios a GitHub Copilot

La colaboración con Copilot no es unidireccional. Puedes mencionarlo en comentarios de la PR o en comentarios en línea del código, igual que a otro miembro del equipo. Copilot verá el comentario e iniciará otra sesión para resolverlo. Los resultados son probabilísticos, así que no hay una indicación exacta que debas usar. Por ejemplo, puedes pedir que:

- añada encabezados con una breve descripción al principio de cada archivo de código;
- añada comentarios TSDoc a archivos TypeScript y Astro;
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**. Puedes usar una de las ideas anteriores u otra petición.
2. Selecciona **View Session** para ver cómo Copilot inicia una sesión nueva y realiza los cambios.
3. Selecciona **Back to pull request** para volver a la PR.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, aparecerá un commit nuevo en la PR.
5. Selecciona **Files changed** y revisa los cambios.

Puedes seguir iterando hasta que estés conforme. Después, convierte el borrador en una PR lista para revisión y combínala en la rama principal.

![Convertir una PR en lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la funcionalidad de juegos relacionados

Vuelve a la PR que Copilot creó para mostrar juegos relacionados en la página de detalles.

1. Abre el repositorio en GitHub.com y selecciona **Pull Requests**.
2. Abre la PR titulada, por ejemplo, **Show related games on the game details page**.
3. Selecciona **Files changed** y revisa el código generado.
4. Cuando termines, vuelve a **Conversation** y desplázate hacia abajo.
5. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

6. Comprueba que se ejecuten y superen las comprobaciones del proyecto. Puede tardar unos minutos.
7. **Opcional:** cambia a la rama del agente en tu codespace para probar manualmente la funcionalidad. Sustituye `<branch-name>` por el nombre de la rama que creó Copilot, por ejemplo `copilot/fix-8`:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la funcionalidad de juegos relacionados. Puedes pedirle cambios mediante un comentario en **Conversation**, por ejemplo, ajustar el número de juegos o añadir encabezados y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas.

## Revisar las funciones de accesibilidad

Por último, revisa la funcionalidad que implementó el agente personalizado de accesibilidad. La PR debería incluir el modo de alto contraste solicitado en el ejercicio 3 y el modo claro que pediste desde Mission Control en el ejercicio 4.

1. Abre el repositorio en GitHub.com y selecciona **Pull Requests**.
2. Abre la PR titulada, por ejemplo, **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot todavía está trabajando, la PR mostrará la etiqueta **[WIP]**. Espera a que complete la tarea.

3. Selecciona **Files changed** y revisa el código generado. Presta atención a:
   - los controles de interfaz para cambiar de modo;
   - el uso del almacenamiento local para conservar las preferencias;
   - los cambios de estilo de los modos de alto contraste y claro;
   - los atributos de accesibilidad, como las etiquetas ARIA y la navegación con teclado;
   - el código JavaScript o TypeScript que gestiona los modos.
4. Vuelve a **Conversation** y desplázate hacia abajo.
5. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

6. Comprueba que se ejecuten y superen las comprobaciones del proyecto.
7. **Opcional:** cambia a la rama en tu codespace para probar manualmente la accesibilidad. Sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

   Inicia la aplicación y prueba los controles de alto contraste y modo claro en el navegador. Comprueba que las preferencias se conserven al recargar la página.

Observa cómo el agente de accesibilidad ayudó a aplicar prácticas recomendadas. Si encuentras algún problema o mejora posible, menciona a **@copilot** en un comentario para pedir cambios, como hiciste con las otras PR.

## Ejercicio opcional: seguir delegando

El agente en la nube resulta especialmente útil cuando puede ocuparse de tareas reales del backlog mientras tú avanzas en otras cosas. Practica este flujo creando algunas incidencias y asignándolas a Copilot. Por ejemplo:

- crear un formulario para expresar interés en la página de detalles del juego;
- implementar la paginación en la página de la lista de juegos;
- añadir validación de entradas y gestión de errores a los auxiliares de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- **Has revisado los archivos de instrucciones personalizados** del repositorio para observar más adelante su efecto en el agente.
- **Has asignado incidencias al agente en la nube de Copilot** y observado cómo prepara el entorno, planifica y ejecuta el trabajo de forma asíncrona.
- **Has creado y usado un agente personalizado** para añadir modos de alto contraste y claro.
- **Has usado la página de agentes de Copilot como centro de control** para supervisar y guiar la sesión de accesibilidad.
- **Has revisado e iterado sobre las PR del agente en la nube**, mencionando a `@copilot` para pedir cambios y aprobando los flujos de trabajo.

## Repaso y próximos pasos

Has completado el recorrido del agente en la nube. Si quieres seguir explorando, los demás recorridos amplían lo que has practicado:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración con MCP desde el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades, un agente de QA, Playwright MCP, Agent Merge y una integración opcional con el agente en la nube mediante `/delegate`.

En tu propio repositorio, prueba estas ideas:

- asigna al agente una incidencia de refactorización o cobertura de pruebas;
- crea un agente personalizado para otro ámbito;
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube de Copilot][agent-firewall]

---

| [← Lección anterior: gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
