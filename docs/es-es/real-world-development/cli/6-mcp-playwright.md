---
title: "Lección 6 - Valida la funcionalidad con Playwright MCP"
description: "Inspecciona o configura Playwright MCP en Copilot CLI y úsalo para explorar en un navegador la característica de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos explicado, programar no consiste únicamente en escribir código. También hay que trabajar con datos y servicios externos, e incluso proporcionar a Copilot automatizaciones adicionales. Para ello sirven los servidores MCP: permiten que Copilot vaya más allá de las funciones integradas en la CLI y acceda a más herramientas y servicios.

En esta lección:

- entenderás qué es el Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI;
- añadirás el servidor MCP de Playwright si aún no está disponible;
- pedirás al agente que controle un navegador y explore la característica de filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar los cambios en la interfaz de usuario hay que interactuar con ella. Queremos que Copilot pueda utilizar el sitio web como lo haría una persona y automatizar mejor el proceso de cambios, para tener más confianza en que las actualizaciones funcionan según lo previsto.

## ¿Qué es el Model Context Protocol (MCP)?

El [Model Context Protocol (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos. Gracias a MCP, los agentes pueden acceder a información actualizada y realizar acciones en tu nombre.

Los agentes acceden a estas herramientas y recursos mediante un servidor MCP, que actúa como puente entre el agente de IA y los servicios externos. Cada servidor MCP representa un conjunto distinto de herramientas y recursos a los que puede acceder el agente.

Algunos servidores MCP conocidos son:

- **[GitHub MCP Server][github-mcp]**: proporciona acceso a las API para gestionar repositorios, incidencias y solicitudes de cambios de GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: proporciona funciones de automatización del navegador mediante Playwright.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro de MCP][mcp-registry] para facilitar la búsqueda y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de utilizar uno, revisa su código fuente, comprueba quién lo publica y valora las implicaciones de seguridad.

## Añade el servidor MCP de Playwright

Si el servidor MCP de Playwright aún no está disponible, añádelo para que Copilot pueda interactuar con el sitio como lo haría una persona.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos y continúa con la sección siguiente.

1. Vuelve a tu Codespace.
2. En Copilot CLI, escribe este comando para abrir el cuadro de diálogo para añadir un servidor MCP:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Confirma que el tipo de servidor sea **STDIO** pulsando <kbd>Enter</kbd> y, después, pulsa <kbd>Tab</kbd>.
5. Pega lo siguiente en el cuadro de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo de MCP.

## Pide a Copilot que explore la característica con Playwright

Antes has comprobado manualmente que la funcionalidad funciona. Ahora utilizaremos el servidor MCP de Playwright disponible en Copilot para hacer lo mismo.

1. Envía este prompt para pedir a Copilot que utilice el servidor MCP de Playwright y valide la funcionalidad:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> No es necesario pedirle explícitamente a Copilot que utilice el servidor MCP, ya que normalmente lo detecta por sí solo. Sin embargo, como sabes qué herramienta debe utilizar, puedes indicárselo para obtener resultados más coherentes y ahorrar algunos tokens.

2. Observa cómo Copilot enumera los distintos pasos que realiza en el navegador para comprobar que la funcionalidad funciona.
3. Lee el informe y confirma que todo se comporta según lo previsto.

Copilot iniciará el servidor, utilizará Playwright para interactuar con el sitio, detendrá el servidor y te proporcionará un informe.

## Resumen y siguientes pasos

¡Enhorabuena! Has utilizado el servidor MCP de Playwright para explorar la característica en un navegador real desde Copilot CLI. En resumen:

- has aprendido qué es el Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI;
- has añadido el servidor MCP de Playwright si aún no estaba disponible;
- has pedido al agente que controle un navegador y explore la característica de filtrado.

A continuación, [crearás un agente personalizado de QA][next-lesson] que combinará la habilidad y las herramientas del navegador en un rol especializado.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
