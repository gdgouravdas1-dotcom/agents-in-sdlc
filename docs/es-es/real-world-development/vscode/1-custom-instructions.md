---
title: "Ejercicio 1: instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: requisitos previos][previous-lesson] · [Siguiente lección: modo agente →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información de fondo, asegúrate de que tenga acceso a ella. Los [archivos de instrucciones][instruction-files] proporcionan directrices sobre cómo generar código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de código y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos de instrucciones limitados a rutas;
- generarás la primera parte de los datos de filtrado (un auxiliar de editores) con las instrucciones actuales;
- añadirás un estándar general a `.github/copilot-instructions.md`;
- repetirás la misma indicación y observarás cómo el código generado adopta el nuevo estándar;
- confirmarás las instrucciones y la primera parte del filtrado en `main` para que Copilot pueda usar las pautas actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir todos los estándares. Copilot es probabilístico y no determinista. El objetivo es observar el cambio de tendencia tras actualizar las instrucciones, no obtener exactamente el mismo resultado carácter por carácter.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene directrices de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación mediante comentarios TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que explique su función.

Usarás archivos de instrucciones para que Copilot pueda seguir estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias, como las convenciones de código, las bibliotecas y el estilo de comentarios. Puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos.

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio y contiene información general del proyecto.
- `.github/instructions/*.instructions.md` permite definir instrucciones para lenguajes, tipos de archivo o tareas específicas.

> [!NOTE]
> En el IDE, los archivos de instrucciones se usan para generar código en Copilot Chat, no para completar código ni sugerir la siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube usan las instrucciones del repositorio y los archivos `*.instructions.md` con metadatos `applyTo`.
>
> Copilot también admite otros estándares, como AGENTS.md y CLAUDE.md. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support].

### Prácticas recomendadas para gestionar instrucciones

- Mantén `copilot-instructions.md` centrado en información general: qué se está creando, la estructura del proyecto y las pautas globales de código.
- Usa archivos `*.instructions.md` para instrucciones específicas de tareas o tipos de archivo, como pruebas unitarias, componentes Astro y la capa de datos.
- Escribe en lenguaje natural y explica las directrices con claridad. Incluye ejemplos del código esperado y del que se debe evitar.

No hay una única forma de usar IA. Experimenta para descubrir qué funciona mejor en tu proyecto.

> [!TIP]
> Todo proyecto con GitHub Copilot debería tener un conjunto sólido de instrucciones. En este proyecto hay archivos para varias tareas, como [actualizar la interfaz de usuario][ui-instructions] y [trabajar con Astro][astro-instructions].
>
> Copilot también puede generar estos archivos. Las opciones varían según la interfaz; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI.
>
> Para encontrar plantillas, consulta [awesome-copilot][awesome-copilot], un repositorio de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizados del proyecto

El repositorio incluye un archivo principal `copilot-instructions.md` y archivos `*.instructions.md` para tareas específicas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Examina la descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas de **GitHub Actions Workflows**. Se aplican a las interacciones con Copilot.
3. Explora `.github/instructions` y las instrucciones para Astro, Drizzle, pruebas y otros temas.
4. Abre `.github/instructions/unit-tests.instructions.md` y revisa el campo `applyTo`, que define el patrón glob aplicable a los archivos, por ejemplo `**/*.test.ts`.
5. Observa las instrucciones específicas para las pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y revisa sus enlaces a otras instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares del proyecto, pero todavía no exige documentación en el código. Añadirás los comentarios TSDoc y los encabezados de archivo en los siguientes pasos.

## Crear una rama para los cambios

1. Vuelve al codespace.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar Copilot Chat antes de actualizar las instrucciones

> [!TIP]
> **Abrir Copilot Chat**
>
> En el codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían según el ejercicio; cada paso los indicará cuando sea necesario.

Primero enviarás una indicación con las instrucciones actuales. Después de actualizarlas, repetirás la misma indicación para comparar el resultado.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot solo reciba el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe estar el auxiliar.
3. En el selector de agentes de la vista Chat, selecciona **Agent**.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía esta indicación:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará cambios, normalmente en el archivo auxiliar y sus pruebas.
6. Observa que la función propuesta usa tipos, recibe un cliente `db` como primer argumento y devuelve una lista tipada de editores. Estas convenciones proceden de `.github/instructions/drizzle.instructions.md`, aplicable a `src/lib/*.ts`.
7. Fíjate en que el código propuesto **no incluye** comentarios TSDoc ni un encabezado de archivo.

> [!CAUTION]
> Como Copilot es probabilístico, puede que añada comentarios aunque no se lo hayas pedido. No pasa nada: lo importante es observar la mejora de coherencia después de actualizar las instrucciones.

## Añadir un estándar general para el repositorio

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Localiza **Code formatting requirements**, cerca de la línea 27. Aún no hay una regla de documentación del código, por eso el auxiliar generado no incluía comentarios.
3. Añade estas líneas Markdown debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Las instrucciones de `.github/copilot-instructions.md` son el lugar adecuado para estándares que se aplican a todo el proyecto, como la regla de comentarios de documentación que acabas de añadir.

## Repetir la indicación y observar el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat**.
2. Abre `src/lib/publishers.ts`.
3. Envía la misma indicación:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Comprueba que la función también incluya un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Has orientado a Copilot para que siga un estándar nuevo y lo aplique al código en el que se basará el siguiente ejercicio.

## Confirmar las instrucciones y enviar la rama

1. Abre un terminal nuevo con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba que los cambios estén presentes:

   ```bash
   git status
   ```

3. Prepara y confirma las instrucciones y todos los archivos del auxiliar que haya cambiado Copilot:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios auxiliares (por ejemplo, pruebas), añádelos también.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envía la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crear y combinar una solicitud de incorporación de cambios

Con la rama enviada, crea una PR y asóciala a la incidencia del backlog sobre los estándares de documentación. Copilot puede localizar la incidencia, crear la PR y combinarla.

> [!NOTE]
> En este ejercicio Copilot actuará por primera vez en GitHub. La plantilla del proyecto ya conecta las herramientas de GitHub con tu espacio de trabajo. Puede que VS Code te pida iniciar sesión la primera vez que Copilot use una; sigue las instrucciones. Aprenderás más sobre esta conexión, Model Context Protocol, en otro ejercicio.

1. Abre Copilot Chat en el codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista y selecciona **Agent**.
3. Pide a Copilot que busque la incidencia sobre las instrucciones y cree una PR desde la rama actual, indicando que también se ha añadido el auxiliar de editores para el filtrado:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Si Copilot solicita permiso para realizar acciones en GitHub, revisa el comando y selecciona **Allow** cuando corresponda.
5. Cuando se cree la PR, pide a Copilot que la combine y vuelva a `main`:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Revisa las acciones y los comandos de terminal que Copilot solicita y selecciona **Allow** cuando corresponda.

¡Has creado y combinado una PR con ayuda de GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones y has usado Copilot Chat en VS Code para:

- enviar una indicación de generación de código y observar el resultado con las instrucciones actuales;
- añadir un estándar general a `.github/copilot-instructions.md`;
- repetir la indicación y observar cómo se aplica el nuevo estándar;
- confirmar las instrucciones y el auxiliar para que el siguiente ejercicio pueda aprovecharlos.

A continuación, usarás el [modo agente][next-lesson] para añadir una funcionalidad a distintas partes del código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas][copilot-instructions-five-tips]
- [Instrucciones personales personalizadas para GitHub Copilot][personal-instructions]
- [Awesome Copilot: instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
