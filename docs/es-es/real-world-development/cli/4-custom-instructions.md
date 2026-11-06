---
title: "Lección 4 - Guiar a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, conviene proporcionar a Copilot las indicaciones necesarias. Los [archivos de instrucciones][instruction-files] describen tanto el código que quieres como la forma de estructurarlo. Ahora que has creado el filtrado, explorarás las instrucciones que ha seguido Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de rutas.
- actualizarás el archivo de instrucciones para asegurar que se siguen los estándares de programación.
- observarás cómo afectan al código los archivos de instrucciones.

## Escenario

Como cualquier empresa de desarrollo, Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo. Entre ellos:

- Los comentarios deben explicar la intención y las decisiones que no sean evidentes, no limitarse a repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar su propósito, parámetros y valores devueltos mediante TSDoc/JSDoc, incluido el argumento `db` inyectable cuando exista.
- Los componentes reutilizables de Astro deben documentar sus contratos `Props`, y los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Se deben conservar las directrices existentes de formato y lint.

Con los archivos de instrucciones, proporcionarás a Copilot la información adecuada para que trabaje de acuerdo con estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para que comprenda mejor tu estilo y tus requisitos de programación. Puedes especificar convenciones, bibliotecas e incluso los comentarios que prefieres. Es posible crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`: un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información del proyecto relevante para la mayoría de las solicitudes.
- `.github/instructions/*.instructions.md`: archivos con directrices para lenguajes, tipos de archivo o tareas concretos.

> [!NOTE]
> La compatibilidad con otros formatos de instrucciones varía según el entorno. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explora los archivos de instrucciones personalizadas del proyecto

El proyecto inicial ya incluye varios archivos de instrucciones. Antes de hacer cambios, explora los existentes para ver su efecto.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces (no en el terminal), abre `.github/copilot-instructions.md`.
3. Explora el archivo y fíjate en la breve descripción del proyecto y las directrices de programación. Estas instrucciones se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y explora sus archivos. Hay instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y otros elementos.
5. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior: define el patrón glob que determina a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y fíjate en los enlaces a otros archivos de instrucciones y a los archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y señalar a Copilot ejemplos que puede seguir.

## Actualiza los archivos de instrucciones según las directrices del equipo

Los archivos actuales son un buen punto de partida, pero falta una directriz. Modifica el archivo principal `copilot-instructions.md` para pedir comentarios TSDoc en el nuevo código TypeScript.

1. En `.github/copilot-instructions.md`, localiza la sección **Code formatting guidance**, que debería estar cerca de la línea 35.
2. Añade el siguiente elemento al final de la lista:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Aplica las directrices actualizadas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Después de guardarlas, cierra y reanuda la conversación de filtrado para que Copilot aplique las directrices nuevas sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit` para cerrarla. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio, en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está activa la rama `game-filters-cli`.
3. Pide a Copilot que aplique las directrices actualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Escribe `/diff` y lee los archivos TypeScript modificados. Observa los comentarios TSDoc añadidos y confirma que describen el código con precisión.

## Resumen y pasos siguientes

Has explorado cómo Copilot CLI obtiene contexto de los archivos de instrucciones y has aplicado un estándar nuevo a la funcionalidad. En concreto:

- exploraste cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de rutas.
- actualizaste el archivo de instrucciones para asegurar que se siguen los estándares de programación.
- observaste el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable quality-checks][next-lesson] para que el lint y las pruebas se ejecuten de forma uniforme.

## Recursos

- [Añadir instrucciones personalizadas a Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
