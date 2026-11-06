---
title: "Ejercicio 2 - Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan problemas de deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidades que llevan tiempo en el backlog por falta de recursos. El agente en la nube de GitHub Copilot está diseñado para actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, genera una solicitud de incorporación de cambios (PR) en borrador para que la revise una persona desarrolladora. Así se pueden delegar tareas tediosas, acelerar el desarrollo y permitir que las personas desarrolladoras se centren en cuestiones de mayor alcance.

Con el agente en la nube de Copilot explorarás:

- cómo personalizar el entorno de generación de código.
- cómo asegurarte de que las operaciones se realicen de forma segura.
- la importancia de delimitar bien las incidencias.
- cómo asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica pendiente. Las personas contratistas que crearon la primera versión del sitio dejaron la documentación en un estado deficiente: no hay documentación. Como primer paso, el equipo quiere añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la exploración de juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego, para que las personas puedan seguir explorando. Por ahora no necesita una implementación refinada; quiere algo que sirva para realizar pruebas de aceptación de la experiencia. El trabajo está bloqueado, pero hay otras incidencias con mayor prioridad.

Estas tareas suelen quedar relegadas y son buenas candidatas para asignarlas al agente en la nube de Copilot. El agente puede trabajar en ellas de forma asíncrona mientras el equipo se ocupa de otras tareas y, más adelante, vuelve para revisar el resultado.

## Presentación del agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a una persona desarrolladora. Hay varias formas de asignarle trabajo, como [asignarle una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se le asigna una incidencia, Copilot crea una PR en borrador para hacer seguimiento del progreso, prepara el entorno y empieza a trabajar. Puedes consultar la sesión de Copilot mientras está en curso o una vez finalizada. Cuando la solución esté lista para que la revises, Copilot te mencionará en la PR.

## La importancia de delimitar bien las instrucciones

GitHub Copilot no hace magia: no puedes saltarte el análisis. Incluso las tareas que parecen sencillas pueden ser complejas al examinarlas en detalle. Por eso, [delimita con cuidado las tareas que asignas al agente en la nube de Copilot][cloud-agent-best-practices]. Trátalo como a un asistente de programación con IA: trabaja por etapas, aprende, experimenta y adapta el proceso. Los fundamentos del desarrollo de software no cambian al añadir IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores se han presentado las instrucciones personalizadas y cómo guían a Copilot. Si ya has completado un ejercicio práctico sobre instrucciones personalizadas, habrás visto cómo funciona `.github/copilot-instructions.md`; si no, aprovecha para echarle un vistazo. Antes de asignar trabajo al agente en la nube, revisa los archivos de instrucciones que ya contiene el repositorio. Así podrás reconocer su efecto más adelante.

Abre estos archivos en la interfaz web de GitHub o en un codespace si ya tienes uno en marcha:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation` en la sección siguiente, observa si la PR incluye comentarios TSDoc, convenciones de TypeScript y encabezados de comentario. Estos elementos proceden de los archivos de instrucciones; volveremos a mencionarlos al revisar las PR en un ejercicio posterior.

## Configura el entorno de desarrollo del agente en la nube de Copilot

Escribir código suele requerir un entorno específico y la ejecución de algunos scripts de configuración para garantizar que todo esté preparado. Lo mismo ocurre al asignar tareas a Copilot, que trabaja de forma parecida a un miembro del equipo de desarrollo.

El agente en la nube usa [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo con un [flujo de trabajo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece a trabajar. Así puede acceder a las herramientas y dependencias necesarias. Este flujo de trabajo ya está configurado para facilitar el taller. Proporciona Node.js, las dependencias del proyecto, los navegadores de Playwright para las pruebas de extremo a extremo y la base de datos SQLite migrada y precargada de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
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

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

El flujo de trabajo se parece a cualquier otro archivo de GitHub Actions, pero tiene algunos aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. Se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- El activador `workflow_dispatch` permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions** del repositorio. Así puedes comprobar que funciona correctamente sin esperar a que Copilot lo ejecute.

## Añade documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información obsoleta o carecen de ella. Esta deuda técnica suele quedar desatendida, reduce la productividad y dificulta mantener el código o incorporar nuevas personas al equipo. Copilot puede ayudar a crear documentación; por eso, esta es una buena incidencia para el agente en la nube. Trabajará en segundo plano para generar la documentación necesaria y revisarás el resultado en un ejercicio posterior.

1. Abre tu repositorio en github.com en una pestaña nueva del navegador.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue** para crear una incidencia en blanco.
5. Escribe `Code lacks documentation` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

  ![Detalles de la asignación a Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de incorporación de cambios (PR) que se acaba de generar, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si aún no aparece en la lista, espera uno o dos minutos y actualiza la ventana del navegador.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista de tareas puede tardar varios minutos en aparecer en la PR. Copilot está preparando su entorno —ejecutando el flujo de trabajo descrito anteriormente—, analizando el proyecto y definiendo cómo abordar el problema.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate por la cronología de la PR. Deberías ver una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Es posible que tengas que actualizar la página para ver el indicador actualizado.

16. Observa la sesión en directo y el proceso que sigue Copilot para resolver el problema: explora el código y comprende su estado, se detiene a analizar y elegir un plan, y crea código.

El proceso puede tardar varios minutos. Una de las ventajas principales del agente en la nube de Copilot es que trabaja de forma asíncrona, de modo que puedes ocuparte de otras tareas. Aprovecharemos esa característica asignándole otra tarea y, mientras tanto, nos centraremos en añadir funcionalidades a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una de las grandes ventajas del agente en la nube de GitHub Copilot es la posibilidad de dividir el trabajo: puedes ocuparte de unas tareas mientras el agente trabaja en otras. Añadir una sección de juegos relacionados para el equipo de diseño quizá no lleve mucho tiempo, pero podrías dedicar ese tiempo a otras tareas. Vamos a asignársela al agente en la nube de Copilot.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue** para usar la plantilla en blanco.
5. Escribe `Show related games on the game details page` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, verás varios emoji 👀 en el primer comentario de la incidencia. Indican que Copilot está trabajando en ella.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. El agente en la nube funciona de forma parecida a un miembro del equipo de desarrollo: no necesitas supervisarlo continuamente, puedes revisar el resultado cuando termine. Ahora nos centraremos en crear y utilizar agentes personalizados.

## Resumen y pasos siguientes

En esta lección has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignarle incidencias para que trabaje en ellas de forma asíncrona. Copilot puede abordar deuda técnica, crear funcionalidades o ayudar a migrar código entre frameworks.

Has explorado estos conceptos:

- personalizar el entorno para generar código.
- asegurar que las operaciones se realicen de forma segura.
- comprender la importancia de delimitar bien las incidencias.
- asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes crear y usar agentes personalizados. [El agente en la nube de Copilot también puede usar servidores MCP][cloud-agent-mcp] y puede acceder a las instrucciones personalizadas que exploramos en módulos anteriores.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para configurar el flujo de trabajo del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
