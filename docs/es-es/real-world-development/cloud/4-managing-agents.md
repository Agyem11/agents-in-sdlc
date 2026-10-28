---
title: "Ejercicio 4 - Supervisión y gestión de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores has pedido al agente de Copilot en la nube que realice tres tareas diferentes para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente está diseñado para trabajar de forma autónoma y asíncrona, sigue siendo importante poder supervisar estas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de los agentes que tienen solicitudes de cambios abiertas, consultar las operaciones que han realizado e incluso dirigir las sesiones que siguen en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube;
- dirigirás una sesión en curso para pedir una funcionalidad adicional.

## Escenario

Después de pedir al agente que cree un modo de contraste alto, el equipo se dio cuenta de que también sería un buen momento para añadir un modo claro. Como ya se estaba actualizando el estilo del sitio y añadiendo la funcionalidad para cambiar de modo, parecía lógico incluir esta opción. Queremos orientar el trabajo del agente para que añada el modo claro además del modo de contraste alto.

## Revisa las tareas del agente de Copilot en la nube

Vamos a consultar el estado de todas las tareas que se han asignado al agente de Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en el panel principal y en el panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código;
   - generar API para modificar juegos;
   - añadir un modo de contraste alto al sitio.
3. Selecciona una de las tareas en ejecución. Revisa las operaciones que ha realizado Copilot, que pueden incluir:
   - obtener el código del repositorio;
   - crear el entorno de trabajo de Copilot;
   - configurar servidores MCP;
   - realizar varios pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos concretos que aparezcan dependerán del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Observa también el panel de solicitudes de cambios que aparece a la derecha. Permite consultar la solicitud y los archivos modificados para seguir el trabajo.

## Dirige el agente en la nube

Ahora que has visto las tareas activas, pidamos a Copilot que añada un selector para el modo claro mientras trabaja en el modo de contraste alto.

1. Selecciona la sesión relacionada con el modo de contraste alto. El título exacto dependerá del nombre que haya elegido Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en el panel de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado de configurar el entorno y ha empezado a trabajar. Lo sabrás cuando empiecen a aparecer mensajes similares a los que se muestran a continuación.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, añade este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de pantalla de la tarea del agente en la nube en la página de agentes, con el cuadro de diálogo para dirigir una sesión activa de Copilot resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma que lo ha recibido y lo incorpora a su flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud cuando termine el paso en el que estuviera trabajando al recibir tu mensaje.

El proceso tardará varios minutos, así que puedes aprovechar para hacer una pausa y repasar lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro de control para supervisar y orientar las tareas del agente de GitHub Copilot en la nube. Desde allí puedes seguir las tareas activas y completadas, revisar el trabajo que se está realizando e incluso redirigir una tarea en curso para modificar su alcance o añadir indicaciones.

Has explorado estos conceptos:

- usar el centro de control y la página de agentes para supervisar las tareas del agente en la nube;
- redirigir una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot termina de trabajar en las características de accesibilidad, puedes [iterar sobre las solicitudes de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iteración sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
