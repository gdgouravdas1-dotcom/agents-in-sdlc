---
title: "Ejercicio 1: instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: requisitos previos][previous-lesson] · [Siguiente lección: agente en la nube de Copilot →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información de fondo, asegúrate de que tenga acceso a ese contexto. Los [archivos de instrucciones][instruction-files] proporcionan esa orientación para que Copilot entienda no solo qué quieres, sino también cómo debe hacerlo.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de código y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones aplicables a rutas concretas;
- añadirás un estándar general nuevo a `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los recorridos de VS Code y CLI, aquí no compararás una indicación anterior y otra posterior: el agente en la nube trabaja de forma asíncrona con incidencias de GitHub, por lo que resulta más difícil mostrar el impacto en tiempo real. Verás la influencia de este archivo más adelante, al revisar las solicitudes de incorporación de cambios del agente.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene directrices para sus prácticas de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que explique su función.

Usarás archivos de instrucciones para que Copilot tenga el contexto necesario y pueda seguir estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para ayudarle a entender mejor tu estilo de código y tus requisitos. Puedes especificar convenciones de código, bibliotecas y los comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos de archivo y tareas específicos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general pertinente para la mayoría de las solicitudes, como la pila tecnológica, una descripción del proyecto, las prácticas recomendadas y otras directrices globales.
- Los archivos `.github/instructions/*.instructions.md` permiten definir pautas para tareas o tipos de archivo específicos, por ejemplo para TypeScript, Astro, componentes de interfaz de usuario o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones se usan para generar código en Copilot Chat, no para las sugerencias de código ni las sugerencias de edición siguiente.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con metadatos `applyTo`.
>
> Copilot también admite otros estándares, como los archivos AGENTS.md y CLAUDE.md. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support].

### Recomendaciones para gestionar archivos de instrucciones

- Mantén `copilot-instructions.md` centrado en las directrices generales del proyecto, como su descripción, estructura y estándares de código globales.
- Usa archivos `*.instructions.md` para instrucciones específicas de tipos de archivo, como pruebas unitarias, componentes Astro o la capa de datos, o para tareas concretas.
- Escribe instrucciones claras en lenguaje natural e incluye ejemplos de cómo debería ser el código.

No existe una única forma correcta de crear archivos de instrucciones o de usar IA. La experimentación te ayudará a encontrar lo que funciona mejor para tu proyecto.

> [!TIP]
> Todo proyecto que use GitHub Copilot debería tener un conjunto sólido de archivos de instrucciones. En este proyecto verás archivos para varias tareas, como [actualizar la interfaz de usuario][ui-instructions] y [trabajar con Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar estos archivos. Cada interfaz ofrece una opción distinta, como **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección correspondiente te lo indicará.
>
> Para encontrar plantillas y ejemplos, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizadas del proyecto

Este repositorio incluye un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para diferentes tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Examina la breve descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las directrices anidadas de **GitHub Actions Workflows**. Se aplican a todas tus interacciones con Copilot.
3. Explora la carpeta `.github/instructions`. Contiene instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y otros temas.
4. Abre `.github/instructions/unit-tests.instructions.md` y fíjate en el campo `applyTo`, que define un patrón glob relativo a la raíz del repositorio. En este caso, se aplica a archivos de prueba TypeScript como los que coinciden con `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y ve al final. Verás enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así puedes dividir instrucciones amplias en archivos reutilizables y señalar ejemplos. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares de código, pero todavía no exige documentación en el código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y los encabezados de archivo.

## Añadir un estándar nuevo para el repositorio

Añadirás una regla general para que la documentación del código use comentarios TSDoc y encabezados de archivo. El agente en la nube la aplicará a las incidencias que le asignes más adelante.

Antes de editar el archivo, crea una rama para trabajar:

```bash
git checkout -b custom-instructions-cloud
```

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Localiza la sección **Code formatting requirements**, cerca de la línea 27. Explica los estándares de código, pero todavía no exige documentación en el código.
3. Añade estas líneas de Markdown debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto, puedes crear instrucciones en `.github/copilot-instructions.md` para ofrecer directrices globales, o en archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretas. El archivo del repositorio es el lugar adecuado para estándares generales como la regla de comentarios de documentación que acabas de añadir.

## Confirmar, enviar y combinar la actualización de instrucciones

El agente en la nube lee las instrucciones de la rama a la que se dirige la incidencia. En el siguiente ejercicio, Copilot creará una rama desde `main`; por eso, tus cambios deben llegar a `main` para que el agente pueda leerlos.

1. Prepara y confirma el cambio:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envía la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios desde `custom-instructions-cloud` a `main` y combínala. Así el agente podrá leer estas instrucciones al trabajar en las incidencias del siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante el taller, puedes omitir la rama y confirmar el cambio directamente en `main`. El paso de la rama permite practicar el flujo que seguirías en un proyecto real.

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones del proyecto y has añadido un estándar general a `.github/copilot-instructions.md`. Verás cómo se aplica en las solicitudes de incorporación de cambios que el agente genere durante el resto del recorrido.

A continuación, [asignarás tu primera incidencia al agente en la nube de Copilot][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
