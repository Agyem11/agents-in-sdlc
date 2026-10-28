---
title: "Ejercicio 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados y adaptados a tareas o ámbitos concretos de tu flujo de desarrollo. Al definir agentes mediante archivos Markdown en la carpeta `.github/agents` del repositorio, puedes proporcionar a Copilot instrucciones específicas, prácticas recomendadas, patrones de programación y conocimientos del dominio para que realice ciertos tipos de trabajo con mayor eficacia. Los equipos pueden convertir sus conocimientos en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique las directrices de [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown de la carpeta `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo tiene metadatos YAML con al menos los campos `name` y `description`, seguidos de un prompt en Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Diferencias entre los agentes personalizados y las habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen algunos aspectos en común. Ambos se definen principalmente mediante archivos Markdown y explican a una IA cómo realizar ciertas operaciones. La forma más sencilla de distinguirlos es pensar que el **agente personalizado** es quien realiza el trabajo, mientras que las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están diseñados para coordinar habilidades e incluso otros agentes. En este laboratorio, el agente personalizado de accesibilidad revisará y actualizará el sitio de acuerdo con las directrices de accesibilidad; para hacerlo, podría utilizar habilidades como la de flujo de trabajo de solicitudes de cambios o la que ejecuta y gestiona pruebas.

> [!NOTE]
> No existe una única forma correcta de crear un agente personalizado. Como ocurre con cualquier aspecto de la IA, experimenta e itera para encontrar lo que funciona en tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Con los agentes personalizados explorarás lo siguiente:

- cómo se definen;
- cómo asignarles una tarea.

## Escenario

Tailspin Toys se compromete a que todas las personas puedan utilizar su plataforma de financiación colectiva, con independencia de sus capacidades o preferencias visuales. Los comentarios recientes de los usuarios indican que algunas personas tienen dificultades para leer el tema oscuro actual debido al contraste insuficiente entre el texto y el fondo. Para resolver este problema de accesibilidad, el equipo de diseño ha solicitado un modo de contraste alto que se pueda activar y desactivar.

La accesibilidad es fundamental, así que queremos implementarla cuanto antes. Para generar esta funcionalidad, utilizarás un agente personalizado.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisemos su contenido para entender cómo orientará a Copilot.

Vuelve al codespace y revisa el archivo del agente personalizado de accesibilidad:

1. Abre `.github/agents/accessibility.md`.
2. Observa los metadatos YAML con los campos `name` y `description`.

> [!CAUTION]
> Los campos `name` y `description` de los metadatos son obligatorios para los agentes personalizados.

3. A continuación, revisa las secciones que describen:
   - las responsabilidades principales al generar código para un sitio web accesible;
   - las prácticas recomendadas de accesibilidad;
   - ejemplos de código HTML, CSS y JavaScript;
   - errores y problemas habituales.

## Crea y asigna una incidencia

El panel de agentes es el centro de control de todos los agentes del entorno. Puedes asignar tareas al agente de Copilot en la nube, supervisarlas y redirigirlas o proporcionar instrucciones adicionales. Empecemos por asignar a Copilot una tarea para añadir el modo de contraste alto.

1. Ve al repositorio.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue** para crear una incidencia vacía.
5. Escribe `Add high contrast mode to website` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.
9. Selecciona **Accessibility agent** en la lista de agentes personalizados.

    ![Captura de pantalla de la asignación del agente en la nube, con el agente personalizado y la opción de accesibilidad resaltados](../../../_images/ex5-select-custom-agent.png)

10. Selecciona **Assign**.
11. Copilot empezará a trabajar en segundo plano.

## Resumen y siguientes pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes de IA especializados en tareas y ámbitos concretos. Los agentes personalizados permiten convertir los conocimientos y estándares del equipo en agentes reutilizables que orientan a Copilot para que realice ciertos tipos de trabajo con mayor eficacia.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados;
- cómo asignarles una tarea.

Mientras Copilot trabaja en el modo de contraste alto, puedes [supervisar y dirigir la sesión del agente][next-lesson] desde el panel de control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Preparación para utilizar agentes personalizados en una organización][org-custom-agents]
- [Preparación para utilizar agentes personalizados en una empresa][enterprise-custom-agents]

---

| [← Lección anterior: Agente de GitHub Copilot en la nube][previous-lesson] | [Siguiente lección: Supervisión y gestión de agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
