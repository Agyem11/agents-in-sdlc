---
title: "Lección 8 - Crea y combina la solicitud de cambios de la característica"
description: "Revisa los filtros, las instrucciones, la habilidad actualizada, el perfil de QA y las pruebas; después, crea una solicitud de cambios y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, la habilidad actualizada, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es el momento de revisarlo todo y abrir una solicitud de cambios. Combinaste manualmente la solicitud de cambios (PR) de la valoración con estrellas; esta vez, dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la característica, las actualizaciones de instrucciones, la habilidad y el agente de QA en varias solicitudes de cambios. Para agilizar el taller, has mantenido todo el flujo de trabajo de filtrado y calidad en una conversación y una rama, y lo incluirás en una única solicitud.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una solicitud de cambios;
- inspeccionarás todos los cambios de la característica y las pruebas de verificación;
- crearás la solicitud de cambios del filtrado;
- habilitarás Agent Merge solo después de la revisión y confirmarás que se ha combinado la solicitud.

## Escenario

Durante el flujo de trabajo de filtrado, has utilizado Copilot para planificar, implementar y verificar una característica. Tailspin Toys quiere ahora automatizar el trabajo restante de la solicitud, pero mantener la autorización para combinarla bajo el control del desarrollador.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de cambios. Cuando lo habilitas, Copilot resuelve los bloqueos: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y vuelve a aplicar la base cuando hace falta. Después, habilita la combinación automática de GitHub si el repositorio la permite.

Hasta ahora tú mismo has seleccionado **Merge pull request**. Agent Merge puede encargarse de esa responsabilidad. Revisa el trabajo y confirma que está listo antes de habilitar Agent Merge.

## Utiliza Agent Merge para gestionar la solicitud

Ya tienes todo el código. Revisémoslo, creemos la solicitud y dejemos que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo de agentes escribiendo `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una solicitud de cambios nueva con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la solicitud. Cuando todo se haya completado correctamente, la combinará.
7. Confirma que recibes un mensaje de Copilot parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni la falta de permisos. Si encuentra un bloqueo, lee el motivo que indica y realiza manualmente la combinación revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: generación, prueba y validación de código, así como la gestión de la solicitud de cambios. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una solicitud;
- has inspeccionado todos los cambios de la característica y las pruebas de verificación;
- has creado la solicitud de cambios del filtrado;
- has habilitado Agent Merge después de la revisión y confirmado que la solicitud se ha combinado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Recursos

- [Gestionar solicitudes de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
