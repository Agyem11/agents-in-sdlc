---
title: "Lección 4 - Guía a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, asegúrate de que Copilot disponga de las instrucciones necesarias. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que has creado los filtros, explorarás las instrucciones que ha utilizado Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y los archivos aplicables a rutas concretas;
- actualizarás el archivo de instrucciones para que se sigan los estándares de programación;
- verás el efecto de los archivos de instrucciones en el código.

## Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo. Entre ellos:

- Los comentarios deben explicar la intención y las decisiones que no sean obvias, en lugar de repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar su propósito, parámetros y valores devueltos mediante TSDoc/JSDoc, incluido el argumento `db` inyectable cuando exista.
- Los componentes reutilizables de Astro deben documentar el contrato `Props`, y los comentarios deben seguir siendo correctos cuando cambie el código relacionado.
- Se deben conservar las directrices de formato y lint existentes.

Con los archivos de instrucciones, te asegurarás de que Copilot disponga de la información adecuada para completar las tareas de acuerdo con estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Son una herramienta eficaz para orientar a Copilot y obtener sugerencias y fragmentos de código más útiles. Puedes especificar las convenciones de programación, las bibliotecas y los tipos de comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos de archivo y tareas concretas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto que sea relevante para la mayoría de las solicitudes.
- Los archivos `.github/instructions/*.instructions.md` proporcionan directrices para lenguajes, tipos de archivo o tareas concretos.

> [!NOTE]
> La compatibilidad con otros formatos de instrucciones varía según el entorno. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explora los archivos de instrucciones personalizadas de este proyecto

El proyecto inicial ya incluye varios archivos de instrucciones. Explorémoslos antes de hacer cambios para observar su efecto.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces (no en el terminal), abre `.github/copilot-instructions.md`.
3. Explora el archivo y fíjate en la breve descripción del proyecto y en las directrices de programación. Se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y explora los archivos. Verás instrucciones para archivos de Astro, la capa de datos de Drizzle, pruebas y otros elementos.
5. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` de la parte superior: define un patrón global que determina a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y observa los enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así puedes dividir conjuntos grandes de instrucciones en archivos más pequeños y reutilizables, y señalar ejemplos que Copilot puede seguir.

## Actualiza los archivos de instrucciones para reflejar las directrices del equipo

Los archivos existentes son un buen punto de partida, pero falta algo. Modificaremos el archivo principal `copilot-instructions.md` para que Copilot añada comentarios TSDoc al código TypeScript que genere.

1. En `.github/copilot-instructions.md`, busca la sección **Code formatting guidance**, que debería estar cerca de la línea 35.
2. Añade este elemento al final de la lista:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Aplica las directrices actualizadas

Copilot CLI carga las instrucciones del repositorio cuando se inicia una conversación. Después de guardar las instrucciones actualizadas, cierra y reanuda la conversación de filtrado para que Copilot las utilice sin perder el contexto de la característica.

1. Si la conversación de filtrado sigue abierta, escribe `/exit` para cerrarla. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio, en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está seleccionada la rama `game-filters-cli`.
3. Pide a Copilot que aplique las directrices actualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Observa los nuevos comentarios TSDoc y comprueba que describen el código con precisión.

## Resumen y siguientes pasos

Has explorado cómo Copilot CLI utiliza el contexto de los archivos de instrucciones y has aplicado un nuevo estándar a tu característica. En concreto:

- has explorado cómo llegan al agente las instrucciones del repositorio y los archivos aplicables a rutas concretas;
- has actualizado el archivo de instrucciones para que se sigan los estándares de programación;
- has visto el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable quality-checks][next-lesson] para garantizar que se ejecutan lint y las pruebas de forma coherente.

## Recursos

- [Añade instrucciones personalizadas para Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: una colección de archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
