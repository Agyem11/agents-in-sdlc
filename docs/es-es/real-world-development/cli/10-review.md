---
title: "Lección 10 - Resumen y siguientes pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI a lo largo de un flujo de trabajo continuo en Tailspin Toys. Has:

- preparado un Codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado precargada;
- añadido valoraciones con estrellas, revisado el resultado en un navegador con reenvío de puertos y combinado manualmente tu primera solicitud de cambios (PR);
- partido de la incidencia de filtrado, definido el enfoque en el modo Plan, implementado la característica en el modo Autopilot y revisado el resultado en el modo Interactive;
- orientado al agente con instrucciones personalizadas, personalizado la habilidad `quality-checks` existente y utilizado esa habilidad para ejecutar pruebas unitarias, lint y comprobaciones de tipos;
- añadido el servidor Model Context Protocol (MCP) de Playwright y utilizado la herramienta para explorar el filtrado en un navegador real;
- creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas en el navegador;
- revisado todos los cambios del filtrado y autorizado a Agent Merge a combinar la solicitud;
- explorado comandos de barra para gestionar el contexto y los modelos, compartir sesiones y delegar opcionalmente en la nube.

## Lo que has publicado

El taller incluye dos hitos de solicitudes de cambios, cada uno en su propia rama creada desde `main` actualizado:

1. **Valoraciones con estrellas:** mostrar la valoración existente `starRating` y un estado explícito para los juegos sin valorar en las tarjetas.
2. **Filtrado y flujo de trabajo de calidad:** implementar el filtrado, actualizar las instrucciones y aplicarlas a la característica, personalizar el informe `quality-checks`, crear un perfil de QA e incluir las pruebas asociadas.

Desde la planificación del filtrado hasta la apertura de su solicitud de cambios, has utilizado la misma conversación y rama. Hemos agrupado el trabajo en una solicitud para agilizar el taller.

## Diferentes tipos de verificación

Has comprobado el código de varias maneras: con pruebas automatizadas, inspeccionándolo en el navegador y pidiendo a Copilot que lo explorara mediante MCP. La habilidad `quality-checks` ejecutó las pruebas unitarias, lint y las comprobaciones de tipos, y comunicó los resultados con el nuevo formato. El agente de QA agrupó esos resultados con una revisión de los requisitos y la cobertura antes de abrir la solicitud.

Las pruebas nuevas deben cubrir carencias reales; si el agente de QA no necesita añadir pruebas, el resultado también puede ser correcto. Revisa el código y las pruebas antes de autorizar la combinación, y vuelve a generar las pruebas afectadas después de hacer cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller has actualizado las instrucciones, personalizado una habilidad, creado un perfil de QA y configurado un servidor MCP. Reutiliza estas personalizaciones en distintas conversaciones y ajústalas a las necesidades del equipo. Las instrucciones establecen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios reales y los resultados de las herramientas; no te limites al resumen del agente.

Adapta el **modo y el modelo** a la tarea. Utiliza **Plan** para definir el enfoque antes de programar, **Interactive** para seguir de cerca los cambios concretos y **Autopilot** para tareas bien delimitadas. Elige un modelo más rápido para ediciones rutinarias y uno más avanzado para tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* cambia de forma significativa el resultado.

## Más cosas que explorar

Has completado el flujo de trabajo principal. Estas funciones de CLI también pueden ser útiles:

- `/review` para pedir al agente de revisión de código que analice los cambios;
- `/rubber-duck` para explicar un problema y obtener otra perspectiva;
- `/fleet` para coordinar tareas secundarias independientes en paralelo;
- `/worktree` para aislar una tarea distinta;
- `/delegate` para enviar una tarea al agente de Copilot en la nube.

## Siguientes pasos

La mejor forma de mejorar con cualquier herramienta es seguir utilizándola. Úsala con código de producción, en proyectos personales o para crear esa pequeña aplicación que llevas años imaginando. Comparte lo aprendido con el equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente de Copilot en la nube][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
