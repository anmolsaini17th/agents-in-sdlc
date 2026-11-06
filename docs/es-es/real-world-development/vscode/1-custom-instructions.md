---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información de fondo, debes asegurarte de que pueda acceder a ese contexto. Hay varias formas de proporcionárselo. Una de las principales son los [archivos de instrucciones][instruction-files], que contienen directrices para la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y las instrucciones específicas de rutas.
- generarás la primera parte de la capa de datos para el filtrado (un método auxiliar de editoriales) con las instrucciones actuales.
- añadirás un estándar general al archivo `.github/copilot-instructions.md`.
- volverás a ejecutar el mismo prompt y observarás cómo el código generado sigue el nuevo estándar.
- confirmarás los cambios de instrucciones y la primera parte del filtrado en `main` para que Copilot use las directrices actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir todos los estándares definidos. Copilot no es determinista. El objetivo es observar la tendencia del cambio después de actualizar las instrucciones, no obtener exactamente el mismo código.

## Archivos de instrucciones

### Escenario

Como cualquier empresa de desarrollo, Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo. Entre ellos:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que describa su función.

Con los archivos de instrucciones, proporcionarás a Copilot la información adecuada para que trabaje de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para que comprenda mejor tu estilo y tus requisitos de programación. Puedes especificar convenciones, bibliotecas e incluso los comentarios que prefieres. Es posible crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`: un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información del proyecto relevante para la mayoría de las interacciones con Copilot, como la pila tecnológica, una descripción general del proyecto y las prácticas recomendadas.
- `.github/instructions/*.instructions.md`: archivos con directrices para lenguajes o tipos de archivo concretos, como TypeScript o Astro, o para tareas como crear un componente de interfaz o un conjunto de pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para completar código ni para sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot usan tanto el archivo de instrucciones del repositorio como los archivos `*.instructions.md` con el frontmatter `applyTo`.
>
> Copilot [también admite otros formatos de archivos de instrucciones][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Crear archivos de instrucciones queda fuera del alcance de este taller, pero el proyecto de ejemplo muestra un enfoque representativo. En general:

- Mantén `copilot-instructions.md` centrado en directrices generales del proyecto, como su descripción, estructura y estándares globales de programación.
- Usa archivos `*.instructions.md` para instrucciones específicas de tipos de archivo (pruebas unitarias, componentes Astro, capa de datos) o tareas concretas.
- Escribe en lenguaje natural, con directrices claras y ejemplos del código que se debe usar y evitar.

No existe una única forma correcta de crear archivos de instrucciones, como tampoco la hay de usar la IA. La experimentación te ayudará a encontrar el enfoque adecuado para tu proyecto.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían disponer de un conjunto sólido de archivos de instrucciones. En este proyecto encontrarás instrucciones para varias tareas, como [actualizar la interfaz][ui-instructions] y trabajar con [Astro][astro-instructions].
>
> Copilot también puede generar archivos de instrucciones. Cada entorno ofrece esta función de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección del entorno que estés usando explicará cómo hacerlo cuando corresponda.
>
> Si buscas plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Dedica un momento a leer los archivos de instrucciones del repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas sobre **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot en este repositorio.
3. Abre la carpeta `.github/instructions` y explora sus archivos. Hay instrucciones para Astro, la capa de datos de Drizzle, pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior: define un patrón glob relativo a la raíz del repositorio que determina a qué archivos se aplican las instrucciones. En este caso, coincide con los archivos de prueba TypeScript, por ejemplo, `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos grandes de instrucciones en archivos más pequeños y reutilizables, y proporcionar a Copilot ejemplos que puede seguir. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero aún no requiere documentación en el código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y los encabezados de comentario.

## Crea una rama para los cambios

Sigue las prácticas recomendadas y crea una rama para los cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea y cambia a una rama nueva:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat antes de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea relevante.

Para observar el efecto de las instrucciones personalizadas, empieza enviando un prompt con el archivo de instrucciones actual. Más adelante lo actualizarás y volverás a enviar el mismo prompt para comparar el resultado.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot tenga solo el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ubicarse el método auxiliar.
3. En el menú desplegable de agentes de Chat, selecciona **Agent** para que Copilot pueda cambiar archivos.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explora el proyecto y aplica cambios, a menudo tanto al archivo auxiliar como a sus pruebas.
6. Observa que el método auxiliar propuesto es una función tipada que recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editoriales. Esto procede de las convenciones de la capa de datos definidas en `.github/instructions/drizzle.instructions.md`, que se aplican a `src/lib/*.ts`.
7. Observa que el código propuesto **no incluye** comentarios TSDoc ni un encabezado de comentario para el archivo.

> [!CAUTION]
> Como Copilot es probabilístico, puede que añada comentarios aunque no se lo pidas. No pasa nada: el objetivo es observar la mejora de coherencia después de actualizar las instrucciones.

## Añade un estándar para todo el repositorio

Como se ha indicado, `.github/copilot-instructions.md` proporciona a Copilot información general del proyecto. Añade un estándar de programación para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Localiza la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa cómo documenta los estándares del proyecto, pero aún no incluye reglas para la documentación en el código.
3. Añade estas líneas de Markdown justo después de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto, los archivos de instrucciones pueden estar en el nivel del repositorio (`.github/copilot-instructions.md`) para proporcionar directrices generales o en archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo del repositorio es el lugar adecuado para estándares generales como la regla de comentarios que acabas de añadir.

## Repite el prompt y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el contexto anterior.
2. Vuelve a seleccionar `src/lib/publishers.ts` para que Copilot se centre en el archivo adecuado.
3. Envía el **mismo prompt**:

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

5. Observa que la función propuesta ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Has guiado a Copilot para que siga un estándar nuevo y lo aplique a código real que utilizarás como base en el ejercicio siguiente.

## Confirma las instrucciones y publica la rama

Los archivos de instrucciones son recursos del repositorio y se gestionan mediante el control de versiones, igual que los demás archivos. Confirma los cambios y publica la rama.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba que están presentes las actualizaciones de instrucciones y del método auxiliar:

   ```bash
   git status
   ```

3. Prepara y confirma las instrucciones y todos los archivos modificados por Copilot para crear el método auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama en el repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Ahora que has publicado la rama, crea una solicitud de incorporación de cambios vinculada a la incidencia del backlog sobre los estándares de documentación. Copilot puede buscar la incidencia, crear la PR y asociarla a ella con las herramientas de GitHub conectadas al proyecto.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot actuará en GitHub en tu nombre. La plantilla del proyecto ya configura las herramientas de GitHub; no tienes que preparar nada. La primera vez que Copilot use una herramienta, VS Code podría pedirte que inicies sesión en GitHub. Sigue las instrucciones para autorizarlo. En un ejercicio posterior aprenderás cómo funciona esta conexión mediante Model Context Protocol.

1. Abre Copilot Chat en el codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir Copilot Chat y confirma que **Agent** está seleccionado.
3. Pide a Copilot que encuentre la incidencia sobre las instrucciones y cree una PR desde la rama actual que describa tanto las instrucciones como el nuevo método auxiliar:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Cuando se te solicite **Allow** para que Copilot realice acciones de GitHub en tu nombre, revisa el comando y selecciona **Allow** si corresponde.
6. Cuando se haya creado la PR, pide a Copilot que la combine y vuelva a `main`:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Si se te solicita **Allow** para que Copilot realice acciones de GitHub o ejecute comandos del shell en tu nombre, revisa el comando y selecciona **Allow** si corresponde.

¡Has creado y combinado una solicitud de incorporación de cambios con GitHub Copilot!

## Resumen y pasos siguientes

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones del proyecto y has usado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar el resultado con las instrucciones *existentes*.
- añadir un estándar general al archivo `.github/copilot-instructions.md`.
- volver a enviar el mismo prompt y observar cómo el código propuesto sigue el estándar nuevo.
- confirmar las instrucciones y el método auxiliar en `main` para que el siguiente ejercicio pueda usarlos.

A continuación, aplicarás esas instrucciones en el [modo agente][next-lesson] mientras Copilot añade una funcionalidad al código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
