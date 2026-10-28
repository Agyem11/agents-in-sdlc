---
title: "Ejercicio 3 - Prueba la característica con Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear la característica de filtrado con el modo agente. Antes de abrir una solicitud de cambios, comprueba que funciona en el navegador. En lugar de probar la aplicación manualmente, conectarás el **servidor MCP de Playwright** y pedirás a Copilot que controle un navegador real para probar la característica. Después, publicarás la rama y abrirás la solicitud de cambios.

En este ejercicio:

- aprenderás qué es el Model Context Protocol (MCP) y cómo los servidores MCP amplían las herramientas de Copilot;
- añadirás el servidor MCP de Playwright al espacio de trabajo;
- pedirás a Copilot que lo utilice para probar manualmente la característica en un navegador;
- publicarás la rama y abrirás una solicitud de cambios para el trabajo de filtrado.

## ¿Qué es el Model Context Protocol (MCP)?

El modo agente resulta mucho más útil cuando puede acceder a elementos fuera del editor. El Model Context Protocol (MCP) permite que Copilot se comunique con herramientas y servicios externos.

![Diagrama del funcionamiento interno del modo agente y su interacción con el contexto, el modelo de lenguaje y las herramientas, incluidas las que proporcionan los servidores MCP y las extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

El [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Los agentes acceden a estas herramientas y recursos mediante un servidor MCP, que actúa como puente y gestiona la comunicación con los servicios externos, como API existentes o herramientas locales, por ejemplo, paquetes de NPM. Cada servidor MCP representa un conjunto distinto de herramientas y recursos.

Algunos servidores MCP conocidos son:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona acceso a API para gestionar repositorios de GitHub, crear repositorios, actualizarlos y administrar incidencias y solicitudes de cambios.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: proporciona herramientas de automatización del navegador con Playwright, como navegar por páginas web, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar la búsqueda y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de utilizar uno, revisa su código fuente, comprueba quién lo publica y valora las implicaciones de seguridad. Utiliza solo servidores de confianza y ten cuidado al conceder acceso a recursos o acciones sensibles.

## Revisa la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu codespace.
2. Deberías ver que ya está configurado un servidor `github`:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

La plantilla del proyecto incluye la entrada `github`, que permitió a Copilot consultar la lista de incidencias en el ejercicio anterior. Utiliza el [servidor MCP remoto de GitHub][remote-github-mcp-server], así que no hay que instalar nada localmente: VS Code se conecta por HTTP y te autenticas con GitHub la primera vez que Copilot utiliza una de sus herramientas.

## Añade el servidor MCP de Playwright

Ahora añadirás un segundo servidor. El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador que puede controlar para probar la característica.

1. Añade una entrada `playwright` junto a `github` en `.vscode/mcp.json`:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor `github` remoto, `playwright` es un servidor **local**: VS Code lo inicia en el equipo mediante `npx @playwright/mcp@latest`. La opción `--headless` indica a Playwright que ejecute el navegador sin una ventana visible, algo necesario en un codespace, donde no hay un escritorio que mostrar.

> [!NOTE]
> Tailspin Toys ya utiliza Playwright para las pruebas de extremo a extremo, por lo que normalmente el navegador necesario ya está instalado. Si Copilot indica más adelante que falta un navegador, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

## Inicia y marca el servidor como de confianza

VS Code inicia los servidores MCP **cuando se necesitan**: la primera vez que Copilot solicita herramientas del servidor, VS Code lo inicia y te pregunta si confías en él.

1. En `.vscode/mcp.json`, verás una acción **Start** encima de la entrada `playwright`. Selecciónala para iniciar el servidor y comprobar que la configuración es correcta.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y confirma que confías en él para que se inicie.

El inicio manual es opcional. Si lo omites, VS Code iniciará el servidor y te pedirá que confirmes si confías en él la primera vez que Copilot necesite sus herramientas.

## Prueba la característica de filtrado

El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador real que puede controlar. En lugar de recorrer la aplicación manualmente, el agente puede abrir una página, navegar por ella, aplicar filtros y leer los resultados. Después, resumirá lo que ha observado. Es una forma rápida de comprobar que la característica se comporta como esperas sin salir de la conversación.

Internamente, el servidor MCP de Playwright utiliza el [árbol de accesibilidad][playwright-mcp-server] de la página en lugar de capturas de pantalla. El agente trabaja con elementos estructurados y etiquetados, como botones, enlaces y elementos de lista, igual que las tecnologías de asistencia. Por eso, una comprobación funcional rápida también ofrece una comprobación básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe la característica de filtrado:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot abrirá un navegador mediante el servidor MCP de Playwright, seguirá los pasos y te comunicará sus resultados. Compara el informe con los criterios de aceptación de la incidencia. Si algo no funciona como esperas, haz preguntas de seguimiento o pide que corrija el código antes de abrir la solicitud de cambios.

> [!NOTE]
> La aplicación debe estar en ejecución en `http://localhost:4321` para realizar esta prueba. Si has detenido el servidor de desarrollo, vuelve a iniciarlo antes de enviar el prompt. La primera vez que Copilot utilice el servidor MCP de Playwright, quizá tenga que descargar un navegador. Si indica que falta uno, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publica la rama y crea una solicitud de cambios

Ya has comprobado que la característica funciona. Ahora puedes abrir una solicitud de cambios (PR) para que el equipo la revise. Primero publicarás la rama `filtering-vscode`.

1. Ve al panel **Source Control** del codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Captura de pantalla del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para insertar la rama en el repositorio.

## Crea la solicitud de cambios

Puedes crear una solicitud desde github.com, la interfaz de línea de comandos de GitHub (CLI) u otros entornos. Como ya estás utilizando GitHub Copilot, pídele que la cree: puede localizar la incidencia correspondiente y vincularla a la solicitud.

1. Ve al panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Confirma que está seleccionado **Agent** en la lista de agentes para que Copilot pueda utilizar las herramientas de GitHub.
3. Pide a Copilot que cree una solicitud:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Si es necesario, selecciona **Continue** para permitir que Copilot recopile información y realice las operaciones. La primera vez que utilice una herramienta de GitHub, quizá debas autenticarte; sigue las instrucciones.
5. Observa cómo Copilot busca entre las incidencias, encuentra la correcta y crea la solicitud.
6. Selecciona el enlace de Copilot para revisar la solicitud, pero **no la combines todavía**.

## Resumen y siguientes pasos

En este ejercicio:

- has aprendido qué es el Model Context Protocol (MCP) y cómo amplían sus servidores las herramientas de Copilot;
- has añadido el servidor MCP de Playwright al espacio de trabajo;
- lo has utilizado para probar manualmente la característica de filtrado en un navegador;
- has publicado la rama y abierto una solicitud de cambios.

Has utilizado un servidor MCP para *probar* una característica, pero MCP es solo una forma de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas concretas, como las revisiones de accesibilidad.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro de MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
