---
title: "Lección 5 - Personalizar y usar una habilidad quality-checks"
description: "Explora la habilidad quality-checks existente, personaliza el formato de su informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste únicamente en escribir código. Ya has comprobado manualmente que funciona y has utilizado archivos de instrucciones para asegurar que sigue los estándares del equipo. ¿Y las pruebas? ¿El lint? ¿El resto de las tareas de integración continua (CI)?

Las **habilidades de agente** son adecuadas para este tipo de tareas. Ayudan a Copilot a entender cómo ejecutarlas correctamente.

En esta lección:

- explorarás la habilidad `quality-checks` existente.
- personalizarás el formato de sus resultados.
- volverás a cargar la habilidad y la ejecutarás.

## Escenario

Tailspin Toys usa la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones reutilizables para tareas, scripts ejecutables y recursos de apoyo que el agente carga cuando los necesita. En esencia, son una carpeta con el nombre de la habilidad que contiene un archivo Markdown `SKILL.md`. El archivo incluye un frontmatter con el nombre y la descripción de la habilidad, una descripción general de sus funciones y directrices sobre cuándo usarla. La carpeta también puede incluir subcarpetas con scripts y otros recursos.

> [!NOTE]
> Las carpetas y los archivos adicionales son opcionales. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md` porque utiliza los comandos existentes del proyecto.

Las habilidades pueden estar en `.github/skills` dentro del proyecto, donde todo el equipo puede utilizarlas, o en la carpeta de habilidades del usuario `~/.copilot/skills`.

## Explora la habilidad

Vamos a explorar la habilidad `quality-checks`, que el equipo de Tailspin Toys creó para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve a tu Codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee el `name` y la `description` de la parte superior. La descripción ayuda a Copilot a decidir cuándo usar la habilidad.
3. Lee las instrucciones y observa cómo guían a Copilot por el proceso de pruebas y lint.
4. Fíjate en que la habilidad aún no tiene una sección **Results output formatting**.

## Ejecuta la habilidad antes de cambiarla

Copilot CLI permite ejecutar habilidades directamente o pedirlo en lenguaje natural. Pide a Copilot que ejecute las tres comprobaciones de la habilidad.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Usa este prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observa el informe final.

## Personaliza el informe

Queremos que el informe indique qué se ha ejecutado, si ha terminado correctamente y qué han comunicado las herramientas. Actualiza la habilidad para generar ese informe.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final del archivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guarda automáticamente.

## Ejecuta la habilidad actualizada

Ahora comprobarás la habilidad modificada. Copilot CLI puede volver a cargar las habilidades editadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Usa exactamente el mismo prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observa el informe final y compáralo con el primero.

## Resumen y pasos siguientes

Has personalizado y usado una habilidad de agente existente. En esta lección:

- exploraste la habilidad `quality-checks` para pruebas unitarias, lint y comprobaciones de tipos.
- personalizaste el formato de sus resultados.
- volviste a cargar la habilidad y la ejecutaste.

Ese cambio acompañará al filtrado en la PR de la funcionalidad. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante el servidor MCP de Playwright][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad sirven de referencia, no son tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación incluido: `drawio`][drawio-example]
- [Pruebas en el navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
