---
title: "Lección 5: personalizar y usar la habilidad quality-checks"
description: "Explora la habilidad quality-checks existente, personaliza el formato del informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Desarrollar código implica más que escribirlo. Ya has comprobado manualmente que funciona y has usado archivos de instrucciones para que siga los estándares. ¿Qué hay de las pruebas, el lint y las demás partes de la integración continua (CI)?

Las **habilidades de agente** son adecuadas para este tipo de tareas. Ayudan a Copilot a ejecutar operaciones siguiendo las pautas pertinentes.

En esta lección:

- explorarás la habilidad `quality-checks` existente;
- personalizarás el formato de los resultados;
- volverás a cargar y ejecutarás la habilidad.

## Escenario

Tailspin Toys usa la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones reutilizables, scripts ejecutables y recursos complementarios que el agente carga cuando los necesita. En esencia, cada habilidad es una carpeta con un archivo Markdown `SKILL.md`. El archivo contiene metadatos con un nombre y una descripción, una explicación de la tarea y pautas para decidir cuándo usarla. También puede incluir carpetas con scripts y otros recursos.

> [!NOTE]
> Una habilidad no necesita archivos adicionales. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md` porque usa los comandos que ya existen en el proyecto.

Las habilidades pueden estar en `.github/skills` para que el equipo las comparta y reutilice, o en la carpeta de habilidades personal `~/.copilot/skills`.

## Explorar la habilidad

Explorarás `quality-checks`, que el equipo de Tailspin Toys creó para ejecutar las pruebas unitarias, lint y las comprobaciones de tipos.

1. Vuelve al codespace y abre `.github/skills/quality-checks/SKILL.md` en el editor.
2. Lee `name` y `description` al principio. La descripción ayuda a Copilot a decidir cuándo usar la habilidad.
3. Lee las instrucciones y observa cómo guían el proceso de pruebas y lint.
4. Fíjate en que todavía no hay una sección **Results output formatting**.

## Ejecutar la habilidad antes de modificarla

1. Vuelve a la conversación del filtrado en modo Interactive.
2. Envía esta indicación:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe final.

## Personalizar el informe

Mejorarás el informe para que indique qué comandos se ejecutaron, si se superaron y qué resultados comunicaron las herramientas.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final del archivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guarda automáticamente.

## Ejecutar la habilidad actualizada

Copilot CLI puede volver a cargar las habilidades editadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Vuelve a usar exactamente la misma indicación:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y usado una habilidad existente. En esta lección:

- has explorado `quality-checks` para las pruebas unitarias, lint y las comprobaciones de tipos;
- has personalizado el formato de los resultados;
- has vuelto a cargar y ejecutado la habilidad.

Este cambio se incluirá en la PR de la funcionalidad de filtrado. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante el servidor MCP de Playwright][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad sirven como referencias, no como tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación: `drawio`][drawio-example]
- [Pruebas del navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
