---
title: "Ejercicio 0: Requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de empezar los ejercicios de VS Code, prepara todo lo necesario. Crearás tu propia copia del repositorio Tailspin Toys, iniciarás un [codespace][codespaces] y comprobarás que GitHub Copilot Chat funciona en el editor.

## Configura el repositorio del laboratorio

Para crear una copia del repositorio con el código que utilizarás, crearás una instancia a partir de la [plantilla][template-repository]. La nueva instancia incluirá todos los archivos necesarios para el laboratorio y la utilizarás durante los ejercicios.

1. En una nueva ventana del navegador, ve a la [plantilla de Tailspin Toys][tailspin-template].
2. Crea tu propia copia del repositorio seleccionando **Use this template** en la página del repositorio del laboratorio. A continuación, selecciona **Create a new repository**.

    ![Botón Use this template](../../../_images/ex0-use-template.png)

3. Si participas en un evento organizado por GitHub o Microsoft, sigue las instrucciones de los mentores. En caso contrario, crea el repositorio nuevo en una organización donde tengas acceso a GitHub Copilot.

    ![Configuración de la plantilla del repositorio](../../../_images/ex0-repository-settings.png)

4. Anota la ruta del repositorio que has creado (**organization-or-user-name/repository-name**), ya que la necesitarás más adelante en el laboratorio.

> [!NOTE]
> **La lista de tareas ya está preparada**
>
> Al crear el repositorio a partir de la plantilla, se generará automáticamente una lista de incidencias de GitHub. Trabajarás con ellas durante todo el taller; no tienes que crear ninguna.

## Crea un codespace

A continuación, utilizarás un codespace para completar los ejercicios del laboratorio.

[GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que permite escribir, ejecutar y depurar código directamente en el navegador. Proporciona un IDE completo con compatibilidad con varios lenguajes de programación, extensiones y herramientas.

1. Ve al repositorio que acabas de crear.
2. Selecciona el botón verde **Code**.

    ![Botón Code](../../../_images/ex0-code-button.png)

3. Selecciona la pestaña **Codespaces** y, después, el botón **+** para crear un codespace.

    ![Creación de un codespace](../../../_images/ex0-create-codespace.png)

La creación del codespace tardará varios minutos, aunque será mucho más rápida que instalar todos los servicios manualmente. Mientras tanto, puedes explorar otras características de GitHub Copilot, que veremos a continuación.

> [!CAUTION]
> Volverás al codespace en un ejercicio posterior. Por ahora, déjalo abierto en una pestaña del navegador.

> [!NOTE]
> Este taller está diseñado para ejecutarse en un codespace o en un [contenedor de desarrollo local][dev-containers]. Ambos entornos incluyen todos los requisitos previos para que puedas seguir el taller sin problemas. Si prefieres trabajar localmente, abre el repositorio clonado en VS Code y selecciona **Reopen in Container** cuando se te solicite. VS Code creará el mismo contenedor de desarrollo que utiliza el codespace.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Usa GitHub Copilot Chat y el modo agente

Para acceder al modo agente de GitHub Copilot Chat, debes tener instalada la extensión GitHub Copilot Chat en el IDE. Si utilizas un GitHub Codespace, ya debería estar instalada.

> [!TIP]
> Si no tienes instalada la extensión GitHub Copilot Chat, puedes [instalarla desde Visual Studio Code Marketplace][copilot-chat-extension]. También puedes abrir la vista Extensions de Visual Studio Code, buscar **GitHub Copilot Chat** y seleccionar **Install**.

Una vez instalada la extensión, es posible que tengas que autenticarte con tu cuenta de GitHub para habilitarla.

1. Vuelve a tu codespace.
2. Si Copilot Chat no aparece a la derecha del editor, selecciona el icono **Copilot Chat** en la parte superior del codespace.
3. Escribe un mensaje como «Hello world» en la ventana de Copilot Chat y pulsa <kbd>Enter</kbd>. Copilot Chat debería activarse.
4. Si aún no te has autenticado, se te pedirá que inicies sesión en tu cuenta de GitHub. Sigue las instrucciones para autenticarte.

    ![Ejemplo del mensaje de autenticación de Copilot Chat](../../../_images/ex1-copilot-authentication.png)

5. Después de autenticarte, debería aparecer la ventana de Copilot Chat.

## Resumen

Enhorabuena: has creado una copia del repositorio del laboratorio. También has iniciado la creación del codespace que utilizarás al empezar a escribir código.

## Siguiente paso

Empecemos a trabajar con Copilot. Continúa con el [ejercicio 1: Instrucciones personalizadas][next-lesson], donde enseñarás a Copilot las convenciones del proyecto.

## Recursos

- [Introducción a GitHub Codespaces][codespaces]
- [Creación de un repositorio a partir de una plantilla][template-repository]
- [Inicio rápido de Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
