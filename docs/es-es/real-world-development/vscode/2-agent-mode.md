---
title: "Ejercicio 2 - Añadir funcionalidades con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios más sencillos de una aplicación suelen requerir modificar varios archivos y realizar tareas como ejecutar pruebas. El flujo de trabajo de una persona desarrolladora suele consistir en localizar los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta, hacer otro cambio y repetir el proceso.

Para eso sirve el modo agente de Copilot.

El modo agente de Copilot trabaja con mayor autonomía en el IDE. Se comporta de forma parecida a una persona desarrolladora: explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Vamos a ver cómo usarlo para añadir funcionalidades al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE y permite ciclos de respuesta rápidos e interacción directa. El agente en la nube funciona de manera asíncrona, como un miembro del equipo, y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás:

- cómo el modo agente de Copilot explora el proyecto, localiza archivos relevantes y coordina cambios.
- cómo puede implementar funcionalidades en la interfaz y la capa de datos.
- cómo revisar los cambios y las pruebas generados por Copilot antes de combinarlos en el código.

## Escenario

A medida que crece la lista de juegos, quieres permitir que se filtren por categoría y editorial. En el ejercicio anterior creaste un método auxiliar para las editoriales; ahora completarás el resto del trabajo de la capa de datos, la interfaz y las pruebas con el modo agente de Copilot.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio para entender sus funciones actuales.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM en su frontmatter; no hay una API de backend independiente ni un framework de interfaz en el cliente. Los métodos reutilizables de acceso a datos se encuentran en `src/lib/`, y la interactividad se implementa con scripts `<script>` de Astro pequeños y acotados que usan las API estándar del DOM.

### Inicia el sitio web

Para facilitar la ejecución, se proporciona el comando `npm run dev`, que inicia el único servidor de desarrollo de Astro. Puedes ejecutarlo en GitHub Codespaces:

1. Vuelve a tu codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Cambia a `main`, actualízala y crea una rama nueva para el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, aparecerá un mensaje parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir un navegador en `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar el enlace localhost en el terminal, se te redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al codespace, que ahora aloja el servidor web.

### Explora el sitio web

Cuando el sitio esté en marcha, explora sus funciones principales:

- **Página de inicio**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea relevante.

El sitio funciona, pero queremos mejorarlo con nuevas funciones. Empecemos por el backlog. Al crear el proyecto a partir de la plantilla, se generó automáticamente un conjunto de incidencias. Pide a GitHub Copilot que te las muestre.

1. En el menú desplegable de agentes de Chat, selecciona **Agent**. El agente planifica e implementa cambios en varios archivos, ejecuta comandos del terminal y utiliza herramientas.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Este taller no recomienda un modelo por encima de otro. Al crear el taller, usamos Claude Sonnet 4.5 y lo incluimos en las instrucciones para que las sugerencias fueran relativamente uniformes. Sin embargo, como los modelos de lenguaje grandes son probabilísticos, es normal que las sugerencias que recibas sean distintas.

> [!NOTE]
> Debido al carácter probabilístico de los modelos de lenguaje grandes, Copilot puede usar un comando MCP diferente, pero debería completar la tarea.

3. Pide a Copilot que muestre el backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado los elementos según lo que considera más útil para las personas usuarias.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Usarás el archivo de instrucciones de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Observa las directrices generales para añadir funciones, entre ellas:
   - una descripción general de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a instrucciones específicas para distintos tipos de archivo, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así se pueden reutilizar directrices, dividir conjuntos complejos en partes más manejables y proporcionar ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado, tendrás que realizar al menos tres cambios independientes en la aplicación:

- añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento del filtrado.
- actualizar la página de listado de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de combinar los cambios. El modo agente de Copilot puede realizar estas tareas.

1. Continúa la conversación actual con Copilot o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones UI](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el menú desplegable.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que el modelo seleccionado sigue siendo **Claude Sonnet 4.5**.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad, tanto de la capa de datos como de la interfaz y las pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos según la complejidad de la tarea y del código. Mientras trabaja, pueden aparecer los botones **Keep** y **Undo** en el editor. Cuando termine, tendrás esas opciones para todos los cambios; no hace falta seleccionarlas mientras el trabajo está en curso.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas y que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea sin detectar errores.

   ![Sesión completa de Copilot Agent Mode](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examina el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o por cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y pasos siguientes

¡Enhorabuena! En este ejercicio has aprendido a usar el modo agente de GitHub Copilot para añadir funciones al sitio de Tailspin Toys. Has visto cómo:

- implementar funciones en la interfaz y la capa de datos con el modo agente de GitHub Copilot.
- explorar el proyecto, localizar archivos relevantes y coordinar cambios.
- revisar los cambios y las pruebas generados antes de combinarlos en el código.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar paginación

A medida que crece la lista de juegos, será necesario habilitar la paginación. Aplica lo aprendido en este ejercicio y pide a Copilot que la implemente en el sitio. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas existentes, incluidos los archivos de instrucciones del proyecto.
- decide cómo quieres implementar la paginación: por ejemplo, si la persona usuaria podrá elegir el tamaño de página o si será fijo.
- incluye en el prompt las indicaciones necesarias para obtener la implementación que quieres.
- puede que tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto. Es lo habitual al trabajar con Copilot.

## Recursos

- [Copilot: modos ask, edit y agent; para qué sirven y cuándo usarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
