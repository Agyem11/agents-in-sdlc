---
title: "Lección 5 - Personalizar y utilizar una habilidad quality-checks"
description: "Explora la habilidad quality-checks existente, personaliza el formato de su informe y úsala para verificar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Desarrollar software implica mucho más que escribir código. Ya hemos comprobado manualmente que el código funciona y utilizado archivos de instrucciones para garantizar que cumple nuestros estándares. Pero ¿qué ocurre con las pruebas, el lint y el resto de la integración continua (CI)?

Para este tipo de tareas, las **habilidades de agente** son la opción ideal. Las habilidades ayudan a Copilot a entender cómo ejecutar correctamente operaciones como estas.

En esta lección:

- explorarás la habilidad `quality-checks` existente.
- personalizarás el formato de sus resultados.
- volverás a cargar la habilidad y la ejecutarás.

## Escenario

Tailspin Toys utiliza la habilidad `quality-checks` para las pruebas unitarias, el lint y la comprobación de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente reúnen instrucciones reutilizables para tareas, scripts ejecutables y recursos auxiliares que un agente carga cuando los necesita. En esencia, son una carpeta con el nombre de la habilidad y un archivo Markdown llamado `SKILL.md`. El archivo Markdown incluye frontmatter con un nombre y una descripción, que definen la habilidad, además de una introducción sobre lo que hace y orientaciones para determinar cuándo debe utilizarse. La carpeta también puede incluir subcarpetas con scripts y otros recursos que la habilidad necesite.

> [!NOTE]
> Una habilidad no necesita archivos ni carpetas adicionales. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md` porque utiliza los comandos existentes del proyecto.

Las habilidades pueden estar en la carpeta `.github/skills` de un proyecto y, así, convertirse en recursos del repositorio que el equipo puede compartir y reutilizar. También pueden estar en la carpeta de habilidades del usuario, `~/.copilot/skills`.

## Explorar la habilidad

Vamos a explorar la habilidad que el equipo de Tailspin Toys ha creado para ejecutar las pruebas unitarias, el lint y las comprobaciones de tipos: `quality-checks`.

1. Vuelve a tu codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee el `name` y la `description` de la parte superior. La descripción ayuda a Copilot a decidir cuándo utilizar la habilidad.
3. Lee las instrucciones y observa cómo guían a Copilot durante las pruebas y el lint.
4. Fíjate en que la habilidad todavía no incluye la sección **Results output formatting**.

## Ejecutar la habilidad antes de cambiarla

Puedes llamar a las habilidades directamente desde Copilot CLI o pedirlas en lenguaje natural. Vamos a pedir a Copilot que ejecute las tres comprobaciones de la habilidad.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Utiliza el siguiente prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Fíjate en el informe final.

## Personalizar el informe

Nos gustaría tener un informe mejor que indique qué se ha ejecutado, si ha terminado correctamente y qué resultados han devuelto las herramientas. Vamos a actualizar la habilidad para generar ese informe.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade la siguiente sección al final del archivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guarda automáticamente.

## Ejecutar la habilidad actualizada

Ya hemos realizado el cambio. Vamos a probarlo. Copilot CLI puede volver a cargar las habilidades editadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Utiliza el mismo prompt de antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Fíjate en el informe final y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y utilizado una habilidad de agente existente. En esta lección:

- has explorado la habilidad `quality-checks` para las pruebas unitarias, el lint y las comprobaciones de tipos.
- has personalizado el formato de sus resultados.
- has vuelto a cargar y ejecutado la habilidad.

Ese cambio acompañará al filtrado en la PR de la funcionalidad. A continuación, permitirás que Copilot interactúe directamente con el sitio mediante el [servidor MCP de Playwright][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad son referencias, no tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y un script de exportación integrado: `drawio`][drawio-example]
- [Pruebas en navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
