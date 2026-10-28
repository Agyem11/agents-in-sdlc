---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que haya muy pocas organizaciones que no tengan problemas con la deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de características que se han quedado en la lista de tareas pendientes por falta de tiempo. El agente de GitHub Copilot en la nube está diseñado para realizar tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, genera un borrador de solicitud de cambios listo para que lo revise una persona. Así puedes delegar tareas tediosas, acelerar el desarrollo y dedicarte a cuestiones de mayor alcance.

En este ejercicio explorarás lo siguiente con el agente de Copilot en la nube:

- personalizar el entorno en el que se genera código;
- asegurarte de que las operaciones se realicen de forma segura;
- comprender la importancia de delimitar claramente las incidencias;
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en un estado poco satisfactorio: comprobarás que no hay documentación. Como primer paso, quieren añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la búsqueda de juegos. Quiere que la página de detalles de cada juego muestre juegos relacionados, es decir, otros títulos de la misma categoría, para que las personas puedan seguir explorando. Por ahora no necesita una implementación refinada; solo quiere algo que pueda utilizar para probar y aceptar la experiencia de usuario. Aunque esta tarea bloquea el trabajo, hay otras incidencias con mayor prioridad.

Estas son tareas que suelen quedar relegadas y que se pueden asignar al agente de Copilot en la nube. El agente puede trabajar en ellas de forma asíncrona mientras tú te ocupas de otras tareas. Después podrás volver a revisar el trabajo de Copilot y comprobar que el resultado es correcto.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Al igual que con una persona, puedes asignarle trabajo de varias maneras, por ejemplo, [asignando una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Una vez asignada, Copilot creará un borrador de solicitud de cambios para registrar el progreso, preparará un entorno y empezará a trabajar. Puedes consultar la sesión mientras sigue activa o después de que termine. Cuando esté lista para revisar la solución propuesta, Copilot te mencionará en la solicitud de cambios.

## La importancia de delimitar bien las instrucciones

GitHub Copilot no hace magia: no puedes dejar de pensar. Incluso las tareas que parecen sencillas pueden ser complejas al analizarlas en detalle, así que [delimita con cuidado el trabajo que asignas al agente de Copilot en la nube][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque según sea necesario. Los fundamentos del desarrollo de software no cambian al incorporar IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas orientan a Copilot. Si ya has completado un ejercicio práctico sobre instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en acción. Si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo al agente de Copilot en la nube, revisa los archivos de instrucciones que incluye el repositorio para que después puedas observar su efecto.

Abre los siguientes archivos en la interfaz web de GitHub o en un codespace, si ya tienes uno en ejecución:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en especial las convenciones de TypeScript y los requisitos de comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en los metadatos `applyTo`, que aplican estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation` en la sección siguiente, comprueba si la solicitud de cambios resultante incluye comentarios TSDoc, sigue las convenciones de TypeScript y añade encabezados de comentarios. Estas indicaciones proceden de los archivos de instrucciones que acabas de revisar. Volveremos a mencionarlas al revisar las solicitudes de cambios en un ejercicio posterior.

## Configura el entorno de desarrollo del agente de Copilot en la nube

Normalmente, para desarrollar código se necesita un entorno específico y algunos scripts de configuración que dejen todo listo. Lo mismo ocurre cuando se asignan tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube utiliza [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo mediante un [flujo de trabajo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece a trabajar. Así puede acceder a las herramientas y dependencias necesarias. Este flujo ya está configurado para el taller y permite que Copilot utilice Node.js, las dependencias del proyecto, los navegadores para las pruebas de extremo a extremo y la base de datos SQLite migrada y precargada de la aplicación Astro única:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Configura los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de los pasos de configuración, por ejemplo, para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas en esos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de frontend y aplicación - Node.js (ahora toda la aplicación utiliza Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga datos iniciales en SQLite local para que las compilaciones y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

Se parece a cualquier otro archivo de flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. Se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de cambios.
- Observa el desencadenador `workflow_dispatch`, que permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions** del repositorio. Es útil para comprobar que funciona correctamente sin tener que esperar a que Copilot lo ejecute.

## Añade documentación

Todo el mundo entiende la importancia de la documentación, pero muchos proyectos tienen información obsoleta o carecen de ella. Este tipo de deuda técnica suele quedar sin resolver, reduce la productividad y dificulta el mantenimiento del código o la incorporación de nuevos desarrolladores. Copilot destaca en la generación de documentación, por lo que esta es una incidencia ideal para asignar al agente en la nube. Trabajará en segundo plano para generar la documentación necesaria. En un ejercicio posterior volverás a revisar su trabajo.

1. Abre una pestaña del navegador y ve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue** para crear una incidencia vacía.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de cambios que acaba de generarse. Su título será parecido a `[WIP]: Code lacks documentation`. Si aún no aparece en la lista, espera uno o dos minutos y actualiza la ventana del navegador.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista de tareas de Copilot puede tardar varios minutos en aparecer en la solicitud de cambios. Copilot está preparando su entorno (ejecutando el flujo de trabajo descrito anteriormente), analizando el proyecto y determinando cómo abordar la tarea.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate por la cronología de la solicitud de cambios. Deberías ver una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la ventana para ver el indicador actualizado.

16. Observa que puedes desplazarte por la sesión en directo y ver cómo Copilot resuelve el problema. Explora el código y comprende el estado del proyecto, observa cómo Copilot se detiene a pensar y decidir el plan adecuado y cómo crea código.

Es probable que el proceso tarde varios minutos. Una de las principales ventajas del agente de Copilot en la nube es que puede trabajar de forma asíncrona y dejarte tiempo para otras tareas. Aprovecharemos esa característica asignándole otra tarea y, mientras tanto, nos ocuparemos de escribir código para añadir funcionalidades a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles del juego

Como ya se ha explicado, una de las principales ventajas del agente de Copilot en la nube es la posibilidad de repartir el trabajo: tú puedes encargarte de una tarea mientras el agente trabaja en otra. Aunque añadir una sección de juegos relacionados para el equipo de diseño quizá no lleve mucho tiempo, puedes dedicar ese tiempo a otras tareas. Vamos a asignársela al agente en la nube.

1. Vuelve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue** para utilizar la plantilla vacía.
5. Escribe `Show related games on the game details page` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, verás unos 👀 en el primer comentario de la incidencia. Indican que Copilot ya está trabajando en ella.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. El agente de Copilot en la nube trabaja de forma parecida a un ingeniero de software: no necesitas supervisarlo continuamente, solo revisar el resultado cuando haya terminado. Ahora vamos a crear y utilizar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que trabaje en ellas de forma asíncrona. Copilot puede ayudarte a resolver deuda técnica, crear funcionalidades o migrar código de un framework a otro.

Has explorado estos conceptos:

- personalizar el entorno en el que se genera código;
- asegurarte de que las operaciones se realicen de forma segura;
- comprender la importancia de delimitar claramente las incidencias;
- asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes crear y utilizar agentes personalizados. El [agente de Copilot en la nube también puede usar servidores MCP][cloud-agent-mcp] y puede acceder a las instrucciones personalizadas que hemos visto en módulos anteriores.

## Recursos

- [Acerca del agente de Copilot en la nube][copilot-agents]
- [Asignación de incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para configurar el flujo de trabajo del agente de Copilot en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
