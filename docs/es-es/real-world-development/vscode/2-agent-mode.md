---
title: "Ejercicio 2 - Añade funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios más sencillos de una aplicación suelen requerir modificar varios archivos y realizar operaciones como ejecutar pruebas. El trabajo habitual de desarrollo incluye localizar los archivos necesarios, hacer cambios, ejecutar pruebas, depurar, descubrir qué archivo se ha pasado por alto y volver a modificarlo. La lista continúa.

Para esto sirve el modo agente de Copilot.

El modo agente de Copilot está diseñado para trabajar de forma más autónoma en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Veamos cómo utilizarlo para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres sean parecidos, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE y permite ciclos de comentarios rápidos e interacción directa. El agente en la nube trabaja de forma asíncrona como un miembro del equipo y se comunica contigo mediante incidencias y solicitudes de cambios.

En este ejercicio aprenderás a:

- utilizar el modo agente de Copilot para explorar el proyecto, identificar archivos relevantes y coordinar cambios;
- implementar características en la interfaz y la capa de datos;
- revisar los cambios y las pruebas generados por Copilot antes de incorporarlos al código.

## Escenario

A medida que crece la lista de juegos, queremos permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior ya has creado una función auxiliar para editoriales; ahora completarás el trabajo pendiente de la capa de datos, la interfaz y las pruebas con el modo agente de Copilot.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, exploremos el sitio para entender su funcionalidad actual.

El sitio es una plataforma de financiación colectiva de juegos de mesa con una temática de desarrollo. Permite mostrar los juegos y sus detalles. Toda la aplicación está implementada con Astro y genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz en el cliente. Las funciones auxiliares reutilizables de acceso a datos se encuentran en `src/lib/`; la interactividad se añade mediante un pequeño `<script>` de Astro con ámbito limitado y las API estándar del DOM.

### Inicia el sitio

La tarea `npm run dev` inicia el único servidor de desarrollo de Astro.

1. Vuelve a tu codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de Astro esté listo, verás un mensaje parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en el navegador, ciérralo seleccionando **x**.

5. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar el enlace localhost desde el terminal se te redirige a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al codespace, que ahora aloja el servidor web.

### Explora el sitio

La página principal muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones. Al seleccionar un juego, se abre una página de detalles con más información, como el título, la descripción, la editorial y la categoría.

## Explora las incidencias con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando corresponda.

La versión inicial del sitio funciona, pero queremos añadir más funcionalidades. Empecemos por revisar la lista de trabajo. La plantilla ya ha creado automáticamente varias incidencias; pide a GitHub Copilot que las muestre.

1. Selecciona **Agent** en la lista de agentes de Chat. Este agente planifica e implementa cambios en varios archivos, ejecuta comandos de terminal y utiliza herramientas.

   ![Captura de pantalla del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores del taller no recomiendan un modelo concreto. Utilizaron Claude Sonnet 4.5 al crear las instrucciones con la esperanza de obtener sugerencias relativamente coherentes y ofrecer una experiencia uniforme. Como los modelos de lenguaje grandes son probabilísticos, es normal que las sugerencias difieran de las que aparecen en el taller.

> [!NOTE]
> Debido a la naturaleza probabilística de los modelos de lenguaje grandes, Copilot puede utilizar un comando MCP diferente y aun así completar la tarea.

3. Envía este prompt para consultar las incidencias pendientes:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para permitir que se ejecute el comando que enumera las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado las incidencias según cuáles cree que serán más útiles para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Utilizarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidad al sitio.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Observa la orientación general sobre cómo añadir funcionalidad, que incluye:
   - una descripción de la arquitectura;
   - principios de diseño de componentes, capacidad de prueba y accesibilidad;
   - enlaces a archivos de instrucciones para tipos de archivo, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden incluir referencias a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así se pueden reutilizar directrices, dividir conjuntos complejos en partes más manejables y proporcionar ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado, habrá que actualizar al menos tres aspectos:

- añadir o ajustar la lógica de filtrado restante en la capa de datos (`src/lib/`);
- añadir o actualizar las pruebas del comportamiento del filtrado;
- actualizar la página con la lista de juegos para incluir la interfaz de filtrado.

Antes de incorporar los cambios, las pruebas deben ejecutarse y superarse. El modo agente de Copilot puede encargarse de estas tareas.

1. Puedes continuar en la conversación actual o seleccionar **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de pantalla de la selección del archivo de instrucciones de interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en la lista de agentes.

   ![Captura de pantalla del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado.
5. Pide a Copilot que implemente la característica según la incidencia relacionada:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la característica, como los asistentes de la capa de datos, la interfaz y las pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Captura de pantalla de Copilot explorando archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot explorará el proyecto, modificará archivos y ejecutará pruebas. El proceso puede tardar varios minutos. Es posible que aparezcan los botones **Keep** y **Undo** en el editor; cuando termine, habrá una opción para aceptar o deshacer cada cambio. No es necesario seleccionarlos mientras el agente sigue trabajando.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de pantalla del cuadro de diálogo de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que haya pausas e incluso que algunas pruebas fallen durante el proceso. Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Captura de pantalla de una sesión completada de Copilot Chat en modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado y busca posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y siguientes pasos

En este ejercicio has aprendido a utilizar el modo agente de GitHub Copilot para añadir funcionalidad al sitio de Tailspin Toys. Has visto cómo:

- implementar características en la interfaz y la capa de datos;
- explorar el proyecto, identificar archivos relevantes y coordinar cambios;
- revisar los cambios y las pruebas antes de incorporarlos al código.

A continuación, [probarás la característica con Playwright MCP][next-lesson] y abrirás una solicitud de cambios.

### Ejercicio de exploración adicional: implementa la paginación

A medida que crezca la lista de juegos, será necesario habilitar la paginación. Utiliza lo aprendido para pedir a Copilot que actualice el sitio. Al planificar el trabajo:

- sigue las prácticas recomendadas y utiliza los archivos de instrucciones existentes;
- decide si los usuarios podrán elegir el tamaño de página o si será fijo;
- proporciona a Copilot la orientación necesaria para obtener la implementación que deseas;
- itera con Copilot y solicita cambios o añade contexto. Así suele trabajarse con Copilot.

## Recursos

- [Modos Ask, Edit y Agent de Copilot: qué hacen y cuándo se utilizan][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
