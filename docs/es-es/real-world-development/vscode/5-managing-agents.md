---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando usas el modo agente de GitHub Copilot, trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como trabaja en tu equipo en tiempo real, Copilot Chat en VS Code muestra cada llamada a herramienta y cada edición a medida que se producen. Puedes revisar las diferencias en línea, aceptar o rechazar cambios concretos y orientar la conversación con prompts de seguimiento, sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás los cambios de archivo propuestos.
- pedirás al agente que añada un control para el modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad que usaste en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha añadido otra solicitud: también quiere un control para el modo claro, para las personas a quienes les resulte demasiado oscuro el tema predeterminado. En vez de iniciar otra conversación, dirigirás la sesión actual para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en línea con las diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a herramienta. Verás lecturas y ediciones de archivos y los comandos del terminal que solicite ejecutar.
3. Cuando proponga una edición, aparecerán las diferencias en línea:
   - selecciona **Keep** para aceptar el cambio.
   - selecciona **Undo** para revertirlo y pedir al agente que lo intente de nuevo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a herramienta requiere permiso, por ejemplo, para ejecutar un comando del terminal, el agente se detendrá y mostrará una solicitud **Allow**. Aprueba únicamente los comandos que te parezcan adecuados.

5. Espera a que termine de implementar el modo de alto contraste. Deberías ver un mensaje final que resuma los cambios.

## Pide al agente que añada el modo claro

Ahora que el modo de alto contraste está implementado, ampliarás la misma conversación con un control para el modo claro. Como continuarás en la misma sesión, el agente conserva el contexto del trabajo anterior: los componentes modificados, el enfoque de estilo y el uso del almacenamiento local.

1. Envía este prompt de seguimiento desde Copilot Chat:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente continuará desde donde lo dejó, explorará los cambios realizados y propondrá los cambios necesarios para el modo claro.
3. Revisa cada cambio nuevo a medida que aparezca, como hiciste con el modo de alto contraste. Usa **Keep** o **Undo** para cada diferencia.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, intenta rediseñar los controles de una forma que no te convence—, detenlo con el botón **Stop** junto al campo de chat y envía un mensaje para aclararlo.

> [!TIP]
> Dirigir una sesión en curso es más rápido que empezar de nuevo. Usa prompts de seguimiento para solicitar ajustes («simplifica el CSS y usa variables CSS en vez de duplicar selectores»), corregir el enfoque («no añadas un tercer control; integra el modo claro en el panel de configuración actual») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisa los cambios

Antes de confirmar el trabajo, revisa rápidamente todos los cambios propuestos por el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en los componentes Astro, los estilos y las pruebas relacionadas.
3. Abre algunos archivos modificados y revisa las diferencias. Confirma que se aplican los patrones de accesibilidad del agente personalizado: atributos ARIA, navegación por teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y pasos siguientes

Has usado el modo agente de Copilot Chat para supervisar y dirigir una sesión en curso: revisaste cada edición, aceptaste o rechazaste cambios y enviaste prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección:

- supervisaste el trabajo del agente en tiempo real.
- aceptaste y rechazaste cambios de archivos concretos.
- dirigiste la sesión en curso con prompts de seguimiento.

En la [siguiente lección][next-lesson], publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los resultados del agente personalizado local.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
