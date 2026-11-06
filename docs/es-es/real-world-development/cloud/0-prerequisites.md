---
title: "Ejercicio 0: Requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de empezar los ejercicios del agente en la nube de Copilot, prepara todo lo necesario. Crearás una copia propia del repositorio Tailspin Toys e iniciarás un [codespace][codespaces] para editar archivos de instrucciones y revisar el trabajo del agente en la nube.

## Configura el repositorio del taller

Para obtener una copia del repositorio donde crearás el código, crearás una instancia a partir de la [plantilla][template-repository]. La nueva instancia incluirá todos los archivos necesarios para el taller y la utilizarás durante los ejercicios.

1. En una nueva ventana del navegador, abre la [plantilla de Tailspin Toys][tailspin-template].
2. Crea una copia del repositorio seleccionando el botón **Use this template** en la página del repositorio del taller. Después, selecciona **Create a new repository**.

    ![Botón Use this template](../../../_images/ex0-use-template.png)

3. Si participas en un evento organizado por GitHub o Microsoft, sigue las instrucciones de los mentores. En caso contrario, puedes crear el repositorio en una organización en la que tengas acceso a GitHub Copilot.

    ![Campos de configuración de la plantilla del repositorio](../../../_images/ex0-repository-settings.png)

4. Anota la ruta del repositorio que has creado (**organization-or-user-name/repository-name**), porque la necesitarás más adelante.

## Crea un codespace

A continuación, usarás un codespace para completar los ejercicios del taller.

[GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que permite escribir, ejecutar y depurar código directamente en el navegador. Proporciona un IDE completo con compatibilidad para varios lenguajes de programación, extensiones y herramientas.

1. Abre el repositorio que acabas de crear.
2. Selecciona el botón verde **Code**.

    ![Botón Code](../../../_images/ex0-code-button.png)

3. Selecciona la pestaña **Codespaces** y, después, el botón **+** para crear un codespace.

    ![Crear un codespace](../../../_images/ex0-create-codespace.png)

El codespace tardará varios minutos en crearse, aunque seguirá siendo mucho más rápido que instalar todos los servicios manualmente. Mientras tanto, puedes explorar otras funciones de GitHub Copilot que veremos a continuación.

> [!CAUTION]
> Volverás al codespace en un ejercicio posterior. Por ahora, déjalo abierto en una pestaña del navegador.

> [!NOTE]
> Este taller está preparado para ejecutarse en un codespace o en un [contenedor de desarrollo][dev-containers] local. Ambos incluyen los requisitos previos necesarios para que el taller funcione correctamente. Si prefieres trabajar en local, abre el repositorio clonado en VS Code y selecciona **Reopen in Container** cuando se te solicite. VS Code creará el mismo contenedor de desarrollo que usa el codespace.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Resumen

¡Enhorabuena! Has creado una copia del repositorio del taller y has iniciado la creación del codespace que usarás mientras trabajas con el agente en la nube de Copilot.

## Siguiente paso

Añade instrucciones personalizadas que seguirá el agente en la nube. Continúa con el [Ejercicio 1: Instrucciones personalizadas][next-lesson].

## Recursos

- [Introducción a GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Guía de inicio rápido de Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
