---
title: "Lección 0 - Requisitos previos"
description: "Crea una copia propia de Tailspin Toys y prepara un GitHub Codespace para el taller de Copilot CLI."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Antes de empezar las lecciones de Copilot CLI, prepara todo lo necesario. Crearás una copia propia del repositorio Tailspin Toys e iniciarás un [codespace][codespaces]. En el siguiente ejercicio, usarás su terminal integrado para instalar y ejecutar Copilot CLI.

En esta lección:

- crearás una copia propia del proyecto Tailspin Toys a partir de la plantilla.
- crearás un Codespace y confirmarás que el proyecto está listo.

## Configura el repositorio del taller

Trabajarás con una copia propia del proyecto Tailspin Toys. Créala ahora a partir del [repositorio de plantilla][tailspin-template]. El nuevo repositorio incluye todos los archivos necesarios para el taller.

1. En una nueva ventana del navegador, abre la [plantilla de Tailspin Toys][tailspin-template].
2. Crea una copia del repositorio seleccionando **Use this template** y, después, **Create a new repository**.
3. Si participas en un evento organizado por GitHub o Microsoft, sigue las instrucciones de los mentores. En caso contrario, crea el repositorio en una organización en la que tengas acceso a GitHub Copilot.
4. Anota la ruta del repositorio que has creado (`organization-or-user-name/repository-name`), porque la necesitarás más adelante en el taller.

> [!NOTE]
> Al crear el repositorio a partir de la plantilla, se generará automáticamente un backlog de incidencias de GitHub. Trabajarás con esas incidencias durante el taller; no tienes que crear ninguna.

Usa una copia nueva de la plantilla del taller. Incluye las instrucciones del repositorio, el código de la aplicación, las pruebas, la habilidad `quality-checks` y el backlog que utilizarás. Si usas una copia antigua, comprueba con la persona que facilita el taller que contiene los archivos necesarios.

## Crea un Codespace

A continuación, usarás un Codespace para completar el taller.

[GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que permite escribir, ejecutar y depurar código directamente en el navegador. Proporciona un editor completo con compatibilidad para varios lenguajes de programación, extensiones y herramientas.

1. Abre el repositorio que acabas de crear.
2. Selecciona **Code**.
3. Selecciona la pestaña **Codespaces** y, después, **Create codespace on main**.
4. Espera a que termine la configuración. La plantilla instalará las dependencias del proyecto, Chromium de Playwright y la base de datos local.
5. Si aparece el mensaje **Do you trust the authors of the files in this folder?**, selecciona **Trust Folder & Continue**.
6. Abre un terminal en la raíz del repositorio e inicia la aplicación:

   ```bash
   npm run dev
   ```

7. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser** y confirma que se carga el sitio de Tailspin Toys.
8. Vuelve al terminal y detén el servidor de desarrollo con <kbd>Ctrl</kbd>+<kbd>C</kbd>.

## Resumen y pasos siguientes

Ya tienes todo listo. En esta lección:

- creaste una copia propia del proyecto Tailspin Toys a partir de la plantilla.
- creaste un Codespace y confirmaste que el proyecto estaba preparado.

A continuación, [instalarás GitHub Copilot CLI][next-lesson] en tu Codespace y te autenticarás con tu cuenta de GitHub.

## Recursos

- [Introducción a GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Guía de inicio rápido de Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
