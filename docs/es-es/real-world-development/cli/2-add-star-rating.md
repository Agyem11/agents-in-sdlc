---
title: "Lección 2 - Añade valoraciones con estrellas: una mejora rápida"
description: "Usa Copilot CLI para hacer un pequeño cambio en las tarjetas de juegos, revísalo en un navegador con reenvío de puertos y combínalo en tu primera solicitud de cambios."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ahora que has instalado Copilot CLI y probado una conversación, es el momento de hacer tu primer cambio en el proyecto. Será pequeño: los datos de los juegos ya incluyen una valoración con estrellas, pero aún no se muestra en las tarjetas de la página principal. Pedirás al agente que la muestre, revisarás el cambio y lo combinarás en tu primera solicitud de cambios.

En esta lección:

- iniciarás una conversación de Copilot centrada en una rama de características;
- pedirás al agente que haga un pequeño cambio en el proyecto;
- revisarás el cambio con `/diff`;
- ejecutarás la aplicación para comprobarlo en un navegador con reenvío de puertos;
- abrirás y combinarás tu primera solicitud de cambios.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración con estrellas, que ya aparece en la página de detalles. Sin embargo, las tarjetas de la página principal solo muestran el título, la categoría, la editorial y la descripción. Para empezar, pedirás al agente que muestre en cada tarjeta la valoración existente: un cambio pequeño e independiente, perfecto para tu primera sesión.

## Anatomía de una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de CLI utiliza el repositorio y la rama de Git que están actualmente seleccionados en el terminal, en lugar de crear un árbol de trabajo independiente. Las conversaciones guardadas te permiten volver a la misma conversación más tarde, mientras que los archivos y la rama siguen siendo el estado habitual de Git en el disco.

En una conversación verás tres elementos: tus prompts y las respuestas del agente, la actividad de las herramientas del agente mientras explora y edita archivos, y los cambios que puedes inspeccionar con `/diff`.

## Inicia una conversación y solicita el cambio

Empecemos una conversación nueva para implementar la característica.

1. Vuelve a tu Codespace.
2. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Si Copilot aún no está abierto, inicia la aplicación con este comando:

   ```bash
   copilot --yolo
   ```

4. Comprueba que se inicia una sesión nueva con el comando de barra `/new` y pulsa <kbd>Enter</kbd>.
5. Usa el siguiente prompt para solicitar el cambio:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot explora el proyecto, localiza los archivos que muestran los detalles de los juegos y crea el código necesario. Ya has añadido una característica con Copilot CLI.

## Revisa las diferencias

Siempre hay que revisar los cambios generados por IA antes de combinarlos, por pequeños que sean. Veamos los cambios directamente en Copilot CLI.

1. Escribe `/diff` e inspecciona todos los archivos modificados.
2. Confirma que la tarjeta muestra la valoración numérica cuando existe y `No rating yet` cuando `starRating` es `null`.
3. Confirma que las pruebas cubren ambos casos.
4. Revisa los resultados de las comprobaciones que ha ejecutado Copilot y pídele que corrija los errores que encuentre.
5. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.

> [!NOTE]
> Copilot, como todas las herramientas de IA generativa, ofrece resultados probabilísticos, no deterministas; por tanto, el código concreto puede variar. Revisa el comportamiento en lugar de esperar una implementación exacta.

## Comprueba los cambios

No basta con leer el código y dar por hecho que funciona. Pidamos a Copilot que inicie el sitio para examinar la interfaz de usuario (UI) actualizada en el navegador abierto mediante Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las tarjetas de los juegos muestran sus valoraciones sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que ha iniciado:

   ```plaintext
   Stop the development server you started.
   ```

## Abre y combina tu primera solicitud de cambios

Ya has creado la característica. Ahora crea una solicitud de cambios (PR) para incorporar el código al proyecto.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Escribe `/pr create`. Copilot CLI insertará la confirmación existente al crear la solicitud y mostrará la URL.
3. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la URL que muestra Copilot CLI para abrir la solicitud.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté todo listo, selecciona **Merge pull request** y confirma la combinación.
6. Vuelve a tu Codespace y sal de Copilot CLI con `/exit`.
7. Actualiza la copia local de `main`:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y siguientes pasos

¡Enhorabuena! Has publicado tu primer cambio con GitHub Copilot CLI. En concreto:

- has iniciado una conversación de Copilot centrada en una rama de características;
- has indicado al agente que haga un pequeño cambio en las tarjetas de juegos;
- has revisado el cambio con `/diff`;
- has ejecutado la aplicación para comprobar la valoración con estrellas en un navegador con reenvío de puertos;
- has abierto y combinado tu primera solicitud de cambios.

A continuación, [partirás de la incidencia de filtrado y utilizarás los modos Plan y Autopilot][next-lesson] para crear una característica más amplia.

## Recursos

- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
