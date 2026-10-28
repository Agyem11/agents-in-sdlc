---
title: "Lección 9 - Comandos de barra de GitHub Copilot CLI"
description: "Explora comandos de barra para gestionar el contexto, seleccionar modelos, compartir sesiones y delegar tareas opcionalmente a la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier herramienta de CLI, GitHub Copilot CLI incluye muchos comandos de barra para interactuar con ella. Estos comandos ofrecen funciones avanzadas, información interna y opciones de configuración adicionales. Ya has utilizado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Veamos otros que también pueden ser útiles.

En esta lección:

- utilizarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación;
- utilizarás `/model` para explorar los modelos disponibles;
- aprenderás cómo `/share` puede exportar o compartir una sesión;
- explorarás comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar al agente en la nube.

## Escenario

Has terminado el flujo de trabajo principal de la CLI. Ahora veremos otras funciones: gestionar el contexto, cambiar de modelo, compartir sesiones y, de forma opcional, delegar trabajo al [agente de Copilot en la nube][about-cloud-agent].

## Explora el contexto de Copilot CLI

En tareas grandes o complejas, puede que alcances el límite de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando es necesario. También puedes inspeccionar o compactar el contexto tú mismo con comandos de barra.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio, si aún no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y cómo se distribuye el contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio disponible.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara el resultado. Si la conversación ya es corta, puede que no notes un cambio importante.

> [!NOTE]
> Copilot CLI compacta el contexto automáticamente a medida que se llena la ventana. Utiliza `/compact` si quieres decidir cuándo hacerlo. Usa `/clear` o `/new` cuando pases a una tarea distinta y quieras iniciar una conversación nueva.

## Elige el modelo

Los distintos modelos tienen diferentes puntos fuertes, y cada desarrollador tiene sus propias preferencias. Copilot CLI permite mostrar los modelos disponibles y seleccionar uno.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información sobre su uso.
3. Conserva el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Comparte una sesión

Colaborar y compartir lo aprendido ayuda a todo el equipo a mejorar el uso de las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartir o publicar un gist de GitHub.

1. Escribe `/help` y revisa las opciones de `/share` de la versión que tienes instalada.
2. Si quieres compartir la sesión, elige el destino adecuado. Por ejemplo, `/share file` crea una exportación local en Markdown.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones de sesiones pueden incluir prompts, respuestas e información del proyecto.

No es obligatorio publicar un enlace o un gist. No publiques contenido del repositorio o de una conversación que el equipo no tenga previsto compartir.

## Opcional: amplía el trabajo o delégalo

Has completado el taller principal. Copilot CLI también ofrece comandos para tareas más amplias:

- `/fleet` puede dividir tareas secundarias independientes entre agentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo de Git aislado para una tarea distinta.
- `/delegate` puede enviar una tarea al agente de Copilot en la nube, que trabaja de forma asíncrona y puede abrir una solicitud de cambios.

Estos comandos son opcionales porque pueden crear árboles de trabajo adicionales o trabajo remoto. Antes de probarlos, inicia una tarea nueva y bien delimitada, y revisa el resultado con tu flujo de trabajo habitual. Si quieres profundizar en el trabajo asíncrono con agentes, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y siguientes pasos

Los comandos de barra de Copilot CLI permiten configurarlo, compartir sesiones y consultar lo que ocurre en segundo plano. En esta lección:

- has utilizado `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación;
- has utilizado `/model` para explorar los modelos disponibles;
- has aprendido cómo `/share` puede exportar o compartir una sesión;
- has explorado comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar al agente en la nube.

Hay más comandos de barra y otras funciones de Copilot CLI que puedes explorar. Para terminar el recorrido, [repasaremos lo aprendido][next-lesson] y veremos los siguientes pasos.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca del agente de Copilot en la nube][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
