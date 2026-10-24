---
title: "Lección 4: guiar Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, asegúrate de que Copilot disponga de esas indicaciones. Los [archivos de instrucciones][instruction-files] explican no solo qué código quieres, sino también cómo debe estructurarse. Ahora que ya has creado el filtrado, explorarás las instrucciones que ha utilizado Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y las instrucciones limitadas a ciertas rutas;
- actualizarás el archivo de instrucciones para cumplir los estándares de código;
- observarás el efecto de las instrucciones en el código.

## Escenario

Tailspin Toys tiene pautas para el desarrollo:

- Los comentarios deben explicar el propósito y las decisiones que no sean obvias, no repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar su propósito, parámetros y valor devuelto con TSDoc/JSDoc, incluido el argumento `db` inyectable, cuando lo haya.
- Los componentes Astro reutilizables deben documentar sus contratos `Props`, y los comentarios deben actualizarse cuando cambie el código relacionado.
- Deben respetarse las directrices existentes de formato y lint.

Usarás archivos de instrucciones para que Copilot aplique estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias, como las convenciones de código, las bibliotecas y el estilo de comentarios. Se pueden definir para todo el repositorio o para tipos de archivo y tareas concretos.

- `.github/copilot-instructions.md` se envía a Copilot para **todas** las solicitudes del repositorio y contiene la información global del proyecto.
- `.github/instructions/*.instructions.md` contiene directrices para lenguajes, tipos de archivo o tareas específicas.

> [!NOTE]
> La compatibilidad con otros formatos de instrucciones varía según el entorno. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explorar los archivos de instrucciones personalizados

El proyecto de inicio ya incluye varios archivos de instrucciones. Explóralos antes de modificarlos:

1. Vuelve al codespace.
2. En el editor de Codespaces, no en el terminal, abre `.github/copilot-instructions.md`.
3. Examina la descripción del proyecto y sus directrices de código. Se aplican a todas las interacciones con Copilot en este repositorio.
4. Explora `.github/instructions`, donde encontrarás instrucciones para Astro, la capa de datos Drizzle, las pruebas y otros temas.
5. Abre `.github/instructions/unit-tests.instructions.md` y observa `applyTo`, el patrón que determina a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y revisa sus enlaces a otros archivos de instrucciones y archivos del proyecto. Las rutas permiten reutilizar instrucciones y señalar ejemplos.

## Actualizar las instrucciones del equipo

Añadirás a `copilot-instructions.md` una regla para incluir comentarios TSDoc en el nuevo código TypeScript.

1. Busca la sección **Code formatting guidance**, aproximadamente cerca de la línea 35.
2. Añade este elemento al final de la lista:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Usar las instrucciones actualizadas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Después de guardar el cambio, sal de la conversación y retómala para que Copilot cargue las instrucciones nuevas sin perder el contexto de la funcionalidad.

1. Si la conversación del filtrado sigue abierta, escribe `/exit`. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio y la rama `game-filters-cli`, retoma la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te pregunta, selecciona la conversación del filtrado y confirma que estás en `game-filters-cli`.
3. Pide a Copilot que aplique las directrices actualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Comprueba que los comentarios TSDoc describen el código correctamente.

## Resumen y siguientes pasos

Has explorado cómo Copilot CLI obtiene contexto de los archivos de instrucciones y has aplicado un nuevo estándar a la funcionalidad. En concreto:

- has explorado las instrucciones del repositorio y las instrucciones limitadas a rutas;
- has actualizado el archivo de instrucciones para respetar los estándares de código;
- has observado el efecto de estas instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable `quality-checks`][next-lesson] para aplicar las comprobaciones de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas a Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
