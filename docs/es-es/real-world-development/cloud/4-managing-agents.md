---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores has pedido al agente en la nube de Copilot que se ocupe de tres tareas distintas para mejorar la experiencia y añadir funcionalidades. Aunque el agente trabaja de forma asíncrona y autónoma, sigue siendo importante poder supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver las tareas de los agentes que tienen solicitudes de incorporación de cambios (PR) abiertas. También puedes explorar las operaciones realizadas y orientar una sesión que esté en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- dirigirás una sesión en curso para solicitar funcionalidad adicional.

## Escenario

Después de asignar al agente la creación de un modo de alto contraste, el equipo ha decidido que también sería un buen momento para añadir un modo claro. Como ya se está actualizando el estilo del sitio y añadiendo un control de alternancia, incluir esta funcionalidad parece razonable. Guiarás al agente para que añada el modo claro además del modo de alto contraste.

## Revisa las tareas del agente en la nube de Copilot

Vamos a consultar el estado de todas las tareas asignadas al agente en la nube de Copilot.

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en los paneles principal e izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso. Revisa las operaciones realizadas por Copilot, que pueden incluir:
   - obtener el código del repositorio.
   - preparar el entorno para Copilot.
   - configurar servidores MCP.
   - realizar distintas tareas para completar el trabajo asignado.

> [!NOTE]
> Los pasos concretos varían según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Observa también el panel de solicitudes de incorporación de cambios de la derecha. Permite consultar la PR y los archivos modificados para seguir el trabajo.

## Dirige al agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que incluya el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en el panel de agentes](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado de preparar el entorno y ha empezado a trabajar. Verás mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarea del agente en la nube con el cuadro para dirigir la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma el prompt y lo incorpora a su flujo de trabajo.

## Deja que Copilot trabaje

Copilot comenzará a trabajar en la tarea actualizada. Incorporará la nueva solicitud al flujo cuando termine el paso que estaba ejecutando al recibir el mensaje.

Como antes, el proceso tardará varios minutos. Es un buen momento para hacer una pausa y repasar lo que has aprendido y explorado hasta ahora.

## Resumen y pasos siguientes

En esta lección has explorado la página de agentes de Copilot, el centro de control para supervisar y dirigir las tareas del agente en la nube de GitHub Copilot. Desde allí puedes hacer seguimiento de las tareas activas y completadas, revisar el trabajo y redirigir las tareas en curso para ajustar su alcance o añadir indicaciones.

Has explorado estos conceptos:

- exploraste el centro de control y la página de agentes para supervisar las tareas del agente en la nube.
- redirigiste una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot completa las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
