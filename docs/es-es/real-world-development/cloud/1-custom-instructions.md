---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente en la nube de Copilot →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información de contexto, debes asegurarte de que pueda acceder a ella. Los [archivos de instrucciones][instruction-files] permiten proporcionar esas directrices para que Copilot entienda no solo *qué* quieres que haga, sino también *cómo* quieres que lo haga.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones limitados por ruta.
- añadirás un nuevo estándar para todo el repositorio a `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los talleres de VS Code y CLI, aquí no ejecutarás un prompt antes y después: el agente en la nube de Copilot trabaja de forma asíncrona en las incidencias de GitHub, por lo que es más difícil comparar su efecto en tiempo real. Verás cómo influyen los archivos de instrucciones más adelante, al revisar las solicitudes de incorporación de cambios (PR) que genere el agente en la nube.

## Archivos de instrucciones

### Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo. Entre ellos:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que describa su función.

Con los archivos de instrucciones, proporcionarás a Copilot la información adecuada para que trabaje de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para que comprenda mejor tu estilo y tus requisitos de programación. Esta función te permite orientar a Copilot para obtener sugerencias y fragmentos de código más pertinentes. Puedes especificar las convenciones de programación, las bibliotecas e incluso los tipos de comentarios que prefieres. También puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`: un único archivo que se envía a Copilot con **todas** las solicitudes del repositorio. Debe contener información general del proyecto pertinente para la mayoría de las interacciones con Copilot, como la pila tecnológica, una descripción general del proyecto, las prácticas recomendadas y otras directrices globales.
- `.github/instructions/*.instructions.md`: archivos que puedes crear para tareas o tipos de archivo concretos. Permiten proporcionar directrices para lenguajes como TypeScript o Astro, o para tareas como crear un componente de interfaz de usuario o un nuevo conjunto de pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para completar código ni para sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot usan tanto el archivo de instrucciones del repositorio como los archivos `*.instructions.md` con el frontmatter `applyTo` al generar código.
>
> Copilot [también admite otros estándares para los archivos de instrucciones][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Explicar en detalle cómo crear archivos de instrucciones queda fuera del alcance del taller. Sin embargo, los ejemplos del proyecto muestran un enfoque representativo. En general:

- Mantén `copilot-instructions.md` centrado en directrices generales del proyecto, como una descripción de lo que se está creando, la estructura del proyecto y los estándares globales de programación.
- Usa archivos `*.instructions.md` para proporcionar directrices específicas para tipos de archivo (pruebas unitarias, componentes de Astro o la capa de datos) o tareas concretas.
- Escribe en lenguaje natural y con claridad. Incluye ejemplos del código que se debe usar y del que se debe evitar.

No hay una única forma correcta de crear archivos de instrucciones, como tampoco la hay de usar la IA. La experimentación te ayudará a encontrar el enfoque adecuado para tu proyecto.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían disponer de un conjunto sólido de archivos de instrucciones. Al explorar los de este proyecto, verás que hay archivos para distintos tipos de tareas, como [actualizar la interfaz de usuario][ui-instructions] y trabajar con [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta función de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección del entorno que estés usando la explicará cuando sea pertinente.
>
> Si buscas plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en este repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas sobre **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot en este repositorio.
3. Abre la carpeta `.github/instructions` y explora sus archivos. Hay instrucciones para Astro, la capa de datos de Drizzle, pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior: define un patrón glob, relativo a la raíz del repositorio, que determina a qué archivos se aplican las instrucciones. En este caso, coincide con los archivos de prueba TypeScript, por ejemplo, `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Fíjate en los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y proporcionar a Copilot ejemplos que puede seguir. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero aún no requiere documentación en el código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y los encabezados de comentario.

## Añade un nuevo estándar para el repositorio

El siguiente paso es la única edición que harás aquí: añadirás una regla general para que la documentación se incluya en el código mediante comentarios TSDoc y un encabezado de comentario para cada archivo. El agente en la nube aplicará esta regla más adelante, cuando trabaje en las incidencias que le asignes durante este taller.

Antes de editar, crea una rama para trabajar. El agente en la nube leerá las instrucciones de la rama a la que se asigne la incidencia, pero así podrás confirmar los cambios de forma ordenada:

1. Abre un terminal en tu codespace y crea una rama nueva:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como se ha indicado, `.github/copilot-instructions.md` proporciona a Copilot información general del proyecto. Añade un estándar de programación para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa cómo documenta los estándares de programación del proyecto, pero todavía no incluye una regla sobre documentación en el código; por eso el método auxiliar generado no tenía comentarios de documentación.
3. Añade las siguientes líneas de Markdown justo después de los estándares existentes. Así indicarás a Copilot que añada encabezados de comentario y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, puedes crear archivos de instrucciones en el nivel del repositorio (`.github/copilot-instructions.md`) para ofrecer directrices generales o usar archivos `*.instructions.md` para idiomas, tipos de archivo o tareas concretos. El archivo del repositorio es el lugar adecuado para estándares generales, como la regla sobre comentarios que acabas de añadir.

## Confirma, publica y combina la actualización de instrucciones

El agente en la nube lee los archivos de instrucciones de la rama a la que se asigna la incidencia. En el siguiente ejercicio asignarás una incidencia a Copilot, que creará una rama desde `main`; por tanto, los cambios de instrucciones deben llegar a `main` para que el agente en la nube los aplique.

1. Prepara los cambios y crea un commit:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Publica la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios desde `custom-instructions-cloud` a `main` y combínala. Así, el agente en la nube leerá estas instrucciones cuando trabaje en las incidencias que le asignes en el siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir los pasos de la rama y confirmar los cambios directamente en `main`. Se incluye la rama para que el taller refleje cómo trabajarías en un proyecto real.

## Resumen y pasos siguientes

Has explorado cómo obtiene contexto Copilot de los archivos de instrucciones del proyecto y has añadido un nuevo estándar para todo el repositorio a `.github/copilot-instructions.md`. Durante el resto de este taller verás cómo se aplica ese estándar en las solicitudes de incorporación de cambios que genere el agente en la nube.

A continuación, [asignarás tu primera incidencia al agente en la nube de Copilot][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
