---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Utiliza Plan para acordar un enfoque, Autopilot para crear filtros a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Hemos empezado por añadir una pequeña característica al proyecto. Sin embargo, los cambios de mayor alcance requieren un proceso más sólido. Por suerte, GitHub Copilot CLI está diseñado para adaptarse al flujo de trabajo de una organización y ayudarnos a crear lo que necesitamos de la manera adecuada. Esta es la primera de varias lecciones en las que seguirás un proceso de desarrollo habitual con agentes: partirás de una incidencia para crear una característica, comprobarás que el código es válido y que la característica se comporta como se espera y, finalmente, la combinarás en el proyecto.

> [!NOTE]
> A medida que desarrolles la característica, seguirás utilizando la misma conversación y rama. Normalmente se usarían distintas ramas o solicitudes de cambios para los diferentes tipos de archivos, pero vamos a tomar un atajo para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación de Copilot desde una incidencia de GitHub;
- definirás los requisitos en el modo Plan;
- implementarás la característica con el modo Autopilot;
- revisarás el código;
- validarás manualmente la característica en un navegador con reenvío de puertos.

Mientras sigues desarrollando esta característica, actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks`, añadirás validación con MCP, crearás un agente de QA y abrirás la solicitud de cambios de la característica.

## Escenario

El catálogo de Tailspin Toys no deja de crecer y los visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del trabajo pendiente describe la característica, pero antes de programar hay que acordar algunos detalles, como la combinación de categorías. Utilizarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA al flujo de desarrollo no cambia los fundamentos. ¡Si acaso, los hace aún más importantes! La mayoría de los desarrolladores sigue un flujo parecido a este:

1. Partir de una incidencia registrada que describa el trabajo que hay que realizar.
2. Crear un plan con lo que se debe desarrollar.
3. Desarrollar y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de cambios (PR).
7. Combinar el código cuando se haya revisado y se haya completado correctamente el proceso de integración continua.

> [!NOTE]
> Los detalles concretos dependen del equipo y de la organización, pero la mayoría de los procesos son variaciones de este enfoque.

Si sigues este enfoque estándar, te aseguras de que el código generado por IA cumpla los requisitos establecidos y pase por el mismo proceso de validación que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla la autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: trabajas con el agente. Este sugiere cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en el modo Plan, revisa el plan y, después, utiliza Autopilot para implementarlo.

## Empieza por la incidencia

Antes de empezar a trabajar en los filtros, vuelve a tu Codespace y comprueba que el repositorio y el terminal están listos.

1. Vuelve a tu Codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que se ha combinado la solicitud de cambios de la valoración con estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para trabajar en los filtros:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa dos veces <kbd>Tab</kbd> para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Utiliza las teclas de flecha para resaltar la incidencia **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

Observa que el prompt ahora empieza por `#7` (o por un número parecido). El carácter `#` permite incluir en el contexto una incidencia o solicitud de cambios (PR) de GitHub.

## Planifica la característica de filtrado

La planificación te permite definir cómo se implementará una característica o se realizará una tarea antes de delegársela a Copilot. Para cualquier trabajo complejo, es recomendable dedicar un rato a la planificación. Cambiemos al modo Plan y pidamos a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador debajo del prompt muestra **Plan**.
2. Después de la referencia a la incidencia que has añadido en el paso anterior, escribe este prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot empezará a elaborar el plan. Primero explorará el proyecto y, después, determinará el mejor enfoque.

3. Es posible que Copilot te pregunte cómo debería funcionar el filtrado. Responde según tus preferencias. ¡No hay respuestas incorrectas!
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para ampliarlo.
5. Desplázate por el plan para revisarlo.
6. Pide a Copilot que revise cualquier parte que no coincida con tus decisiones.

## Aprueba Autopilot

Una vez redactado y revisado el plan, es el momento de implementarlo. Deja que Copilot trabaje con el modo Autopilot.

Con Autopilot, Copilot seguirá iterando sobre el problema hasta que considere que lo ha resuelto.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de tu versión.
2. Confirma que el indicador debajo del prompt muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan acordado, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación de inmediato, así que revisa el plan antes. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar por completadas las comprobaciones.

## Revisa y comprueba la implementación

Una vez generado el código, debes revisarlo antes de combinarlo, igual que harías con cualquier otro código. Revisaremos el código y ejecutaremos el sitio para comprobar que todo funciona.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar en el modo Interactive. Confirma que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y las decisiones tomadas durante la planificación.
4. Cuando hayas revisado el código, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa el resultado de las comprobaciones del proyecto y pide a Copilot que resuelva cualquier error.

## Explora la nueva funcionalidad

De acuerdo, el código parece correcto, pero ¿funciona? Como antes, iniciemos la aplicación y abramos el sitio a través del puerto reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros de categoría y editorial, así como las combinaciones acordadas en el plan.
4. Confirma que el restablecimiento de filtros y el comportamiento sin resultados coinciden con la incidencia y tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo que ha iniciado.

## Resumen y siguientes pasos

Has utilizado distintos modos de conversación para crear y revisar una característica. En esta lección:

- has iniciado una conversación de Copilot desde una incidencia de GitHub;
- has definido los requisitos en el modo Plan;
- has implementado la característica con el modo Autopilot;
- has revisado el código;
- has validado manualmente la característica en un navegador con reenvío de puertos.

A continuación, [verás con más detalle cómo generar código de acuerdo con las prácticas documentadas mediante instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
