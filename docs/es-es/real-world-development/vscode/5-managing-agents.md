---
title: "Ejercicio 5 - Supervisión y gestión de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Cuando utilizas el modo agente de GitHub Copilot, este trabaja de forma autónoma: explora el código base, propone cambios, edita archivos y ejecuta comandos. Como trabaja en tiempo real en tu equipo, Copilot Chat en VS Code muestra cada llamada a una herramienta y cada edición a medida que se realizan. Puedes revisar las diferencias en línea, aceptar o rechazar cambios concretos y orientar la conversación con prompts de seguimiento para mejorar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad;
- revisarás y aceptarás los cambios propuestos en los archivos;
- orientarás al agente durante la sesión para añadir un selector de modo claro junto al modo de contraste alto.

## Escenario

El agente personalizado de accesibilidad que utilizaste en el [ejercicio 4][previous-lesson] está implementando el modo de contraste alto. Mientras trabaja, el equipo de diseño solicita añadir también un modo claro para quienes consideran que el tema predeterminado es demasiado oscuro. En lugar de iniciar otra conversación, orientarás la sesión actual para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en el chat con diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a una herramienta. Verás lecturas de archivos, ediciones y los comandos de terminal que solicita ejecutar.
3. Cuando el agente proponga una edición, las diferencias aparecerán en línea:
   - selecciona **Keep** para aceptar el cambio;
   - selecciona **Undo** para revertirlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para ver todos los archivos que el agente ha modificado durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta solicita permiso (por ejemplo, para ejecutar un comando de terminal), el agente se detendrá y mostrará la opción **Allow**. Aprueba solo los comandos que consideres adecuados.

5. Espera a que el agente termine de implementar el modo de contraste alto. Deberías ver un mensaje final que resuma los cambios realizados.

## Pide al agente que añada el modo claro

Ahora que ya está implementado el modo de contraste alto, amplía la misma conversación para añadir un selector de modo claro. Como continuarás en la misma sesión de chat, el agente conserva el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque de estilo y el método de almacenamiento local.

1. Envía el siguiente prompt en Copilot Chat:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente continuará desde donde lo dejó, explorará los cambios realizados y propondrá los cambios necesarios para añadir el modo claro.
3. Revisa cada nueva edición a medida que aparece, como hiciste con el modo de contraste alto. Utiliza **Keep** o **Undo** en cada diferencia.
4. Si el agente sigue un enfoque que no quieres, por ejemplo, si intenta reorganizar los selectores de una forma que no te gusta, detenlo con el botón **Stop** junto al campo de chat y envíale una aclaración.

> [!TIP]
> Es más rápido orientar una sesión en curso que empezar de nuevo. Utiliza prompts de seguimiento para pedir ajustes («simplifica el CSS: utiliza variables CSS en lugar de duplicar selectores»), corregir el enfoque («no añadas un tercer selector; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos selectores»).

## Revisa los cambios

Antes de confirmar los cambios, revisa rápidamente todo lo que ha modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en los componentes de Astro, los estilos y las pruebas relacionadas.
3. Abre algunos archivos modificados y revisa las diferencias. Comprueba que se aplican los patrones de accesibilidad del agente personalizado: atributos ARIA, navegación con el teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has utilizado el modo agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado cada edición, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección:

- has supervisado el trabajo de un agente en tiempo real;
- has aceptado y rechazado ediciones de archivos concretas;
- has orientado una sesión en curso mediante prompts de seguimiento.

En la [siguiente lección][next-lesson], publicarás los cambios de accesibilidad en una solicitud de cambios y revisarás el trabajo del agente personalizado local durante el laboratorio.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iteración sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
