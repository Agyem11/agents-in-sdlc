---
title: "Ejercicio 5: Iteración sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le has pedido que documente el código, cree una característica de juegos relacionados para que el equipo de diseño pueda iterar sobre ella y añada características de accesibilidad, incluidos modos de contraste alto y claro. Vamos a revisar los cambios de código y, si hace falta, proporcionar comentarios a Copilot para mejorar su trabajo.

### Escenario

Como hemos destacado en varias ocasiones, los fundamentos del diseño de software y DevOps no cambian al incorporar IA generativa. Siempre debemos revisar el código generado y seguir nuestros procesos habituales de DevOps. Con esto en mente, revisemos las sugerencias de GitHub Copilot para crear la documentación, añadir la característica de juegos relacionados e implementar las características de accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y agente de GitHub Copilot en la nube

Como el agente de Copilot en la nube trabaja de forma asíncrona y sin supervisión, se han establecido ciertas restricciones de seguridad para que el proceso siga siendo seguro. Entre ellas:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utilizará para el código.
- El agente en la nube se ejecuta en GitHub Actions, donde crea un entorno independiente y efímero para trabajar.
- Una persona debe aprobar cualquier flujo de trabajo de GitHub Actions antes de que pueda ejecutarse.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empezaremos por explorar la primera solicitud de cambios que ha generado el agente de GitHub Copilot en la nube: añadir documentación al código. Utilizarás la interfaz habitual de solicitudes de cambios de GitHub.com.

> [!NOTE]
> Al explorar la solicitud, puede que veas una advertencia que indica que el cortafuegos ha bloqueado a GitHub Copilot. Es **lo esperado**, ya que Copilot tiene acceso limitado de forma predeterminada a los recursos externos, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el cortafuegos del agente de Copilot en la nube][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes de cambios.
3. Abre la solicitud titulada, por ejemplo, **Add missing documentation** o algo parecido.

> [!NOTE]
> Si Copilot sigue trabajando, la solicitud incluirá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos; mientras tanto, puedes descansar o repasar lo que has aprendido.

4. Cuando la solicitud esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que ahora incluye comentarios TSDoc y otra documentación. Los cambios concretos pueden variar.

    Al revisar los cambios, busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Estas indicaciones proceden de los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.

6. Cuando hayas revisado los cambios y estés conforme, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que algunos flujos de trabajo esperan aprobación.
8. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Aprobación y ejecución de flujos de trabajo](../../../_images/shared-approve-workflows.png)
9. Deberías ver los flujos de trabajo en la cola de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto deberían superarse para la aplicación Astro única. Puede tardar unos minutos.

## Pide cambios a GitHub Copilot

Trabajar con Copilot en una solicitud de cambios no es una interacción unidireccional. También puedes mencionarlo en comentarios de la solicitud, como harías con otra persona del equipo, o en comentarios en línea sobre el código. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no podemos darte un texto prescriptivo. Por ejemplo, podrías pedirle a Copilot que:

- añada al principio de cada archivo de código un encabezado de comentarios que describa brevemente su función;
- añada comentarios TSDoc a archivos de TypeScript y Astro;
- cree un archivo README que describa la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier otra persona. Utiliza una de las ideas anteriores u otra sugerencia relacionada con la documentación del código.
2. Selecciona **View Session** para observar cómo Copilot trabaja. Verás que inicia una sesión nueva para realizar los cambios.
3. Puedes seleccionar **Back to pull request** para volver a la solicitud.

    ![Volver a la solicitud de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine los cambios, verás una nueva confirmación en la solicitud.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Cuando lo estés, puedes marcar la solicitud como lista para revisión y combinarla en la rama principal.

![Marcar la solicitud como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la característica de juegos relacionados

Volvamos a la solicitud de cambios que Copilot ha generado para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la solicitud cuyo título se parezca a **Show related games on the game details page**.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado los cambios y estés conforme, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador que señala que algunos flujos de trabajo esperan aprobación.
7. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Aprobación y ejecución de flujos de trabajo](../../../_images/shared-approve-workflows.png)
8. Deberías ver los flujos de trabajo en la cola de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto deberían superarse para la aplicación Astro única. Puede tardar unos minutos.
9. **Opcional:** también puedes cambiar a esta rama en tu codespace para probar manualmente la característica de juegos relacionados. Ve al codespace, abre el terminal y ejecuta estos comandos (sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la característica de juegos relacionados. Como antes, puedes trabajar de forma iterativa con el agente de Copilot en la nube y solicitar cambios. Por ejemplo, podrías pedirle que modifique cuántos juegos relacionados se muestran o que añada encabezados de comentarios y comentarios TSDoc. Recuerda que esta solicitud se asignó **antes** de que actualizaras las instrucciones personalizadas. Como en el ejercicio anterior, puedes añadir estas solicitudes como comentarios en la pestaña **Conversation**; Copilot las verá e iniciará otra sesión.

## Revisa las características de accesibilidad

Por último, revisaremos las características de accesibilidad implementadas con el agente personalizado de accesibilidad. Esta solicitud debería incluir el modo de contraste alto que asignaste en el ejercicio 3 y el modo claro que pediste desde el panel de control en el ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la solicitud cuyo título se parezca a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la solicitud incluirá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta especial atención a:
   - los componentes de interfaz para cambiar de modo;
   - el uso del almacenamiento local para guardar las preferencias del usuario;
   - los cambios de CSS o de estilo para los modos de contraste alto y claro;
   - los atributos de accesibilidad (etiquetas ARIA, navegación con el teclado, etc.);
   - cualquier código JavaScript o TypeScript que gestione el cambio de modo.

6. Cuando hayas revisado los cambios y estés conforme, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que algunos flujos de trabajo esperan aprobación.
8. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Aprobación y ejecución de flujos de trabajo](../../../_images/shared-approve-workflows.png)
9. Deberías ver los flujos de trabajo en la cola de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto deberían superarse para la aplicación Astro única. Puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en tu codespace para probar manualmente las características de accesibilidad. Ve al codespace, abre el terminal y ejecuta estos comandos (sustituye `<branch-name>` por el nombre de la rama creada por Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los selectores de los modos de contraste alto y claro en el navegador. Comprueba que funcionan y que la preferencia se conserva después de volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ha ayudado a orientar a Copilot para implementar estas características siguiendo las prácticas recomendadas de accesibilidad. Si detectas problemas o posibles mejoras de accesibilidad, puedes mencionar a **@copilot** en un comentario y pedirle que los resuelva, igual que con las solicitudes anteriores.

## Ejercicio opcional: sigue delegando

El agente en la nube es más útil cuando puedes asignarle tareas reales de la lista de pendientes y dedicarte a otras cosas. Para practicarlo, crea varias incidencias en tu repositorio y asígnaselas a Copilot. Por ejemplo:

- crear un formulario de interés para patrocinadores en la página de detalles del juego;
- implementar la paginación en la página con la lista de juegos;
- añadir validación de entradas y gestión de errores a los asistentes de acceso a datos.

## Resumen

Has completado el itinerario del agente en la nube. En estas lecciones:

- **Has revisado los archivos de instrucciones personalizadas incluidos en el repositorio** para comprobar cómo afectan al trabajo posterior del agente.
- **Has asignado incidencias al agente de Copilot en la nube** y has visto cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y utilizado un agente personalizado** de accesibilidad para añadir los modos de contraste alto y claro.
- **Has utilizado la página de agentes de Copilot como centro de control** para supervisar y dirigir la sesión de accesibilidad.
- **Has revisado e iterado sobre las solicitudes de cambios del agente en la nube**, mencionando a `@copilot` para pedir cambios y aprobando los flujos de trabajo.

## Revisión y siguientes pasos

Has completado el itinerario del agente en la nube. Si quieres seguir explorando, los otros itinerarios complementan lo que has practicado:

- 🖥️ **[Itinerario de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración con MCP directamente desde el IDE.
- 💻 **[Itinerario de CLI](../../cli/)**: entrega desde el terminal dos cambios revisados con los modos Plan y Autopilot, habilidades de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y un puente opcional `/delegate` hacia el agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asigna al agente en la nube una incidencia de refactorización o de cobertura de pruebas;
- crea un agente personalizado para otro ámbito;
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignación de incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para configurar el flujo de trabajo del agente de Copilot en la nube][cloud-agent-best-practices]
- [Configuración del cortafuegos del agente de Copilot en la nube][agent-firewall]

---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
