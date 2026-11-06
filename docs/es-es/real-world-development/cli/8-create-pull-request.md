---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa en conjunto el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una misma rama. Ahora los revisarás en conjunto y abrirás una solicitud de incorporación de cambios. Combinaste personalmente la PR de las valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Lo habitual sería dividir la funcionalidad, las actualizaciones de instrucciones, la actualización de la habilidad y el agente de QA en varias PR. Para simplificar el taller, has mantenido todo el flujo de filtrado y calidad en una conversación y una rama, y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- inspeccionarás todos los cambios de la funcionalidad y las pruebas de verificación.
- crearás la PR del filtrado.
- activarás Agent Merge únicamente después de revisar los cambios y confirmarás que la PR se ha combinado.

## Escenario

Durante el trabajo de filtrado, has usado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener la autorización para combinarla bajo el control de la persona desarrolladora.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios. Al activarlo, Copilot resuelve los impedimentos de la PR —corrige comprobaciones de integración continua (CI) fallidas, responde a comentarios de revisión y reorganiza la rama cuando es necesario— y, después, habilita la combinación automática de GitHub si el repositorio lo permite.

Hasta ahora, has seleccionado personalmente **Merge pull request**. Agent Merge puede encargarse de esa tarea. Revisa el trabajo y confirma que está listo antes de activarlo.

## Usa Agent Merge para gestionar la PR

Ya has creado todo el código. Ahora lo revisarás en conjunto, crearás la PR y dejarás que Agent Merge se ocupe del resto.

1. Vuelve a tu Codespace.
2. Escribe `/agent` para abrir el cuadro de diálogo de agentes.
3. Selecciona **Default** en la lista de opciones y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará la integración continua de la PR. Cuando todas las comprobaciones se superen, la combinará.
7. Comprueba que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si el proceso se bloquea, lee el motivo y, cuando el repositorio lo permita, realiza manualmente la combinación después de revisarla.

## Resumen y pasos siguientes

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la validación, y ahora el flujo de la solicitud de incorporación de cambios. En esta lección:

- aprendiste qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- inspeccionaste todos los cambios de la funcionalidad y las pruebas de verificación.
- creaste la PR del filtrado.
- activaste Agent Merge después de revisar los cambios y confirmaste que la PR se había combinado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
